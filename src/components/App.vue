<template>
  
    <div>
      <canvas ref="canv" id="cv" :width="width" :height="height"></canvas>          
    </div>
  
</template>

<script>
export default {
  props:["width", "height"],
  data() {
    return {
      
    }
  },
  methods: {
    draw_plus_btn(width,height,triangle_box_width,triangle_width,triangle_height,center_margin,flame_weight,line_width,line_weight,context,plus_color){
      context.clearRect(width-triangle_box_width, 0, triangle_box_width, height*0.5);
      //＋ボタンの枠
      context.beginPath();
      context.moveTo(width-triangle_box_width*0.5, height*0.5-center_margin*0.5-triangle_height); 
      context.lineTo(width-triangle_box_width*0.5-triangle_width*0.5, height*0.5-center_margin*0.5); 
      context.lineTo(width-triangle_box_width*0.5+triangle_width*0.5, height*0.5-center_margin*0.5);
      context.closePath();	
      context.lineWidth = flame_weight;
      context.strokeStyle = "rgb(0,0,0)"; 
      context.stroke();
      context.fillStyle = plus_color;
      context.fill();

      //＋ボタンの"＋"の縦棒
      context.strokeStyle = "rgb(0,0,0)"; 
      context.stroke();
      context.fillStyle="rgba(0,0,0,1)";
      context.fillRect(width-triangle_box_width*0.5-line_weight*0.5, height*0.5-center_margin*0.5-triangle_height*0.4-line_width*0.5, line_weight, line_width);

      //＋ボタンの"＋"の横棒
      context.strokeStyle = "rgb(0,0,0)"; 
      context.stroke();
      context.fillStyle="rgba(0,0,0,1)";
      context.fillRect(width-triangle_box_width*0.5-line_width*0.5,height*0.5-center_margin*0.5-triangle_height*0.4- line_weight*0.5, line_width, line_weight);
    },
    draw_minus_btn(width,height,triangle_box_width,triangle_width,triangle_height,center_margin,flame_weight,line_width,line_weight,context2,minus_color){

      context2.clearRect(width-triangle_box_width, height*0.5, triangle_box_width, height*0.5);
      //-ボタンの枠
      context2.beginPath();
      context2.moveTo(width-triangle_box_width*0.5, height*0.5+center_margin*0.5+triangle_height); 
      context2.lineTo(width-triangle_box_width*0.5-triangle_width*0.5, height*0.5+center_margin*0.5); 
      context2.lineTo(width-triangle_box_width*0.5+triangle_width*0.5, height*0.5+center_margin*0.5);
      context2.closePath();	
      context2.lineWidth = flame_weight;
      context2.strokeStyle = "rgb(0,0,0)"; 
      context2.stroke();
      
      context2.fillStyle = minus_color;
      context2.fill();
   
      //-ボタンの"-"
      context2.strokeStyle = "rgb(0,0,0)"; 
      context2.stroke();
      context2.fillStyle = "rgba(0,0,0,1)";
      context2.fillRect(width-triangle_box_width*0.5-line_width*0.5, height*0.5+center_margin*0.5+triangle_height*0.4-line_weight*0.5, line_width, line_weight);
  
    },
    draw_1_btn(width,height,num_box_width,num_btn_ratio_width,num_btn_ratio_height,side_margin,top_margin,font_size,font,r,flame_weight,context3,num_color){
      context3.clearRect(0, 0, num_box_width, height);
      //1ボタンの枠
      context3.beginPath();
      context3.moveTo(side_margin+r,top_margin);
      context3.arc(side_margin+r, top_margin+r, r, Math.PI*1.5, Math.PI, true);  
      context3.lineTo(side_margin, height-top_margin-r);
      context3.arc(side_margin+r, height-top_margin-r, r, Math.PI, Math.PI*0.5, true); 
      context3.lineTo(num_box_width-side_margin-r,height-top_margin);
      context3.arc(num_box_width-side_margin-r, height-top_margin-r, r, Math.PI*0.5, Math.PI*0, true); 
      context3.lineTo(num_box_width-side_margin,top_margin+r);
      context3.arc(num_box_width-side_margin-r, top_margin+r, r, Math.PI*0, Math.PI*1.5, true); 
      context3.closePath();

      context3.fillStyle = num_color;
      context3.fill();

      //1ボタンの"1"
      context3.font = font;
      context3.fillStyle = "rgba(0,0,0,1)";
      context3.textAlign = "center";
      context3.fillText('5', num_box_width*0.5, top_margin+height*num_btn_ratio_height*0.8,num_box_width*num_btn_ratio_width);
    
      context3.lineWidth = flame_weight;	
      context3.strokeStyle = "rgb(0,0,0)"; 
      context3.stroke();
    },
    click_res(width,height,num_box_width,triangle_box_width,triangle_width,triangle_height,center_margin,side_margin,top_margin,flame_weight,line_width,line_weight,context,context2,context3,plus_color,minus_color,num_color,gradient,gradient2,gradient3,font_size,font,r,num_btn_ratio_width,num_btn_ratio_height){
      let x = 0; //クリックされた場所のキャンバス中のx座標
      let y = 0; //クリックされた場所のキャンバス中のy座標
      let k1 = triangle_height/(triangle_width*0.5)*(-1); //＋ボタンの左斜辺の式の傾き
      let k2 = k1*(-1); //＋ボタンの右斜辺の式の傾き
      let k3 = k2; //-ボタンの左斜辺の式の傾き
      let k4 = k1; //-ボタンの右斜辺の式の傾き
      let b1 = height*0.5-center_margin*0.5-triangle_height - k1*(width-triangle_box_width*0.5); //＋ボタンの左斜辺の式の切片
      let b2 = height*0.5-center_margin*0.5-triangle_height - k2*(width-triangle_box_width*0.5); //＋ボタンの右斜辺の式の切片
      let b3 = height*0.5+center_margin*0.5+triangle_height - k3*(width-triangle_box_width*0.5); //―ボタンの左斜辺の式の切片
      let b4 = height*0.5+center_margin*0.5+triangle_height - k4*(width-triangle_box_width*0.5); //―ボタンの右斜辺の式の切片


      let d = this.$refs.canv;
      //クリックされた座標を取得
      d.onmousedown = (e) => {
        let rect = e.target.getBoundingClientRect();
        x = e.clientX - Math.floor(rect.left);
        y = e.clientY - Math.floor(rect.top);
        //+ボタン
        if(width-triangle_box_width*0.5-triangle_width*0.5<x && x<width-triangle_box_width*0.5+triangle_width*0.5 && height*0.5-center_margin*0.5-triangle_height<y && y<height*0.5-center_margin*0.5 && k1*x+b1<y && k2*x+b2<y) {
          console.log("plus");
          this.$emit('upclick')
          plus_color = "rgba(0,0,0,0.4)";
          this.draw_plus_btn(width,height,triangle_box_width,triangle_width,triangle_height,center_margin,flame_weight,line_width,line_weight,context,plus_color);
        }
        //―ボタン
        else if(width-triangle_box_width*0.5-triangle_width*0.5<x && x<width-triangle_box_width*0.5+triangle_width*0.5 && height*0.5+center_margin*0.5<y && y<height*0.5+center_margin*0.5+triangle_height && y<k3*x+b3 && y<k4*x+b4){
          console.log("minus");
          minus_color = "rgba(0,0,0,0.4)";
          this.draw_minus_btn(width,height,triangle_box_width,triangle_width,triangle_height,center_margin,flame_weight,line_width,line_weight,context2,minus_color);         
        }
        //1ボタン
        else if(side_margin<x && x<num_box_width-side_margin && top_margin<y && y<height-top_margin) {
          console.log("1");
          let gradient6 = context3.createLinearGradient(num_box_width*0.5,top_margin,num_box_width*0.5,height-top_margin);
          gradient6.addColorStop(0.0 , 'rgba(0,0,0,0.5)');
          gradient6.addColorStop(0.8 , 'rgba(0,0,0,0.3)');
          num_color = gradient6;
          this.draw_1_btn(width,height,num_box_width,num_btn_ratio_width,num_btn_ratio_height,side_margin,top_margin,font_size,font,r,flame_weight,context3,num_color);
        }
        //それ以外
        else {
          console.log(`x=${x}, y=${y}`);
        }
      }
      d.onmouseup = (e) => {
        //+ボタン
        plus_color = gradient;
        this.draw_plus_btn(width,height,triangle_box_width,triangle_width,triangle_height,center_margin,flame_weight,line_width,line_weight,context,plus_color);
        //―ボタン
        minus_color = gradient2;
        this.draw_minus_btn(width,height,triangle_box_width,triangle_width,triangle_height,center_margin,flame_weight,line_width,line_weight,context2,minus_color); 
        //1ボタン
        num_color = gradient3;
        this.draw_1_btn(width,height,num_box_width,num_btn_ratio_width,num_btn_ratio_height,side_margin,top_margin,font_size,font,r,flame_weight,context3,num_color);
      }
      d.onmouseout = (e) => {
        //+ボタン
        plus_color = gradient;
        this.draw_plus_btn(width,height,triangle_box_width,triangle_width,triangle_height,center_margin,flame_weight,line_width,line_weight,context,plus_color);
        //―ボタン
        minus_color = gradient2;
        this.draw_minus_btn(width,height,triangle_box_width,triangle_width,triangle_height,center_margin,flame_weight,line_width,line_weight,context2,minus_color); 
        //1ボタン
        num_color = gradient3;
        this.draw_1_btn(width,height,num_box_width,num_btn_ratio_width,num_btn_ratio_height,side_margin,top_margin,font_size,font,r,flame_weight,context3,num_color);
      }
    }
  },
  mounted() {
    let width = this.width; //canvasの横幅
    let height = this.height; //canvasの高さ

    let num_box_ratio = 0.6; //1ボタンのエリアの横幅の割合
    let num_box_width = width*num_box_ratio; //1ボタンのエリアの横幅

    let triangle_box_ratio = 1-num_box_ratio; //三角ボタンのエリアの横幅の割合
    let triangle_box_width = width*triangle_box_ratio; //三角ボタンのエリアの横幅
    let triangle_ratio_width = 0.9; //三角ボタンのエリアの横幅に対する三角ボタンの横幅の割合
    let triangle_ratio_height = 0.3; //三角ボタンのエリアの高さに対する三角ボタンの高さの割合
    let triangle_width = triangle_box_width*triangle_ratio_width; //三角ボタンの横幅
    let triangle_height = height*triangle_ratio_height; //三角ボタンの高さ
    let center_margin = height*0.1; //三角ボタン間の幅
    let flame_weight = width*0.01 //ボタンの枠の太さ

    let line_ratio = 0.5; //三角ボタンの高さに対する＋,-の線の長さの割合
    let line_width = triangle_height*line_ratio; //＋,-の線の長さ
    let line_weight = flame_weight*0.8; //＋,-の線の太さ


    //+ボタンを作成
    let a = this.$refs.canv;
    let context = a.getContext('2d');
    let plus_color;

    let gradient = context.createLinearGradient(width-triangle_box_width*0.5,height*0.5-center_margin*0.5-triangle_height,width-triangle_box_width*0.5,height*0.5-center_margin*0.5);
    gradient.addColorStop(0.0 , 'rgba(0,0,0,0.35)');
    gradient.addColorStop(0.8 , 'rgba(0,0,0,0.05)');
    plus_color = gradient;
    this.draw_plus_btn(width,height,triangle_box_width,triangle_width,triangle_height,center_margin,flame_weight,line_width,line_weight,context,plus_color);
    

    //-ボタンを作成
    let b = this.$refs.canv;
    let context2 = b.getContext('2d');
    let minus_color;

    let gradient2 = context2.createLinearGradient(width-triangle_box_width*0.5, height*0.5+center_margin*0.5+triangle_height, width-triangle_box_width*0.5, height*0.5+center_margin*0.5);
    gradient2.addColorStop(0.0, 'rgba(0,0,0,0.35)');
    gradient2.addColorStop(0.8 , 'rgba(0,0,0,0.05)');
    minus_color = gradient2;
    this.draw_minus_btn(width,height,triangle_box_width,triangle_width,triangle_height,center_margin,flame_weight,line_width,line_weight,context2,minus_color);
    

    let num_btn_ratio_width = 0.75; //1ボタンエリアの横幅に対する1ボタンの横幅の割合
    let num_btn_ratio_height = 0.5; //1ボタンエリアの高さに対する1ボタンの高さの割合
    let side_margin = num_box_width*(1-num_btn_ratio_width)*0.5; //1ボタンエリアと1ボタンの間の横幅
    let top_margin = height*(1-num_btn_ratio_height)*0.5; //1ボタンエリアと1ボタンの間の高さ
    let font_ratio = 0.8; //1ボタンの高さに対するフォントサイズの割合
    let font_size = height*num_btn_ratio_height*font_ratio; //フォントサイズ
    const font = `${font_size}px meirio`; //フォントサイズと書体
    let r = 10; //1ボタンの枠の角の半径
    //1ボタンを作成
    let c = this.$refs.canv;
    let context3 = c.getContext('2d');
    let num_color;

    let gradient3 = context3.createLinearGradient(num_box_width*0.5,top_margin,num_box_width*0.5,height-top_margin);
    gradient3.addColorStop(0.0 , 'rgba(0,0,0,0.35)');
    gradient3.addColorStop(0.8 , 'rgba(0,0,0,0.05)');
    num_color = gradient3;
    this.draw_1_btn(width,height,num_box_width,num_btn_ratio_width,num_btn_ratio_height,side_margin,top_margin,font_size,font,r,flame_weight,context3,num_color);
    //クリックされたら
    this.click_res(width,height,num_box_width,triangle_box_width,triangle_width,triangle_height,center_margin,side_margin,top_margin,flame_weight,line_width,line_weight,context,context2,context3,plus_color,minus_color,num_color,gradient,gradient2,gradient3,font_size,font,r,num_btn_ratio_width,num_btn_ratio_height);

  }
}
</script>

<style scoped>
  #cv {
    width: 100%;
    height: 100%;
  }

</style>