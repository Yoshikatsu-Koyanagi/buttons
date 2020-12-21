  <template>
      <canvas ref="canv" :width="width" :height="height"></canvas>          
</template>

<script>
export default {
  props: ["width", "height","min","max","initial"],
  data() {
    return {
      amount: 0, //個数
      num_box_ratio: 0.6,//数字ボタンのエリアの横幅の割合
      triangle_ratio_width: 0.9, //三角ボタンのエリアの横幅に対する三角ボタンの横幅の割合
      triangle_ratio_height: 0.3, //三角ボタンのエリアの高さに対する三角ボタンの高さの割合
      triangle_margin_ratio: 0.1, //三角ボタン間の高さの三角ボタンのエリア割合
      flame_weight_ratio: 0.01, //キャンバスの横幅に対するボタンの枠の太さの割合
      line_ratio: 0.5, //三角ボタンの高さに対する＋,-の線の長さの割合
      line_weight_ratio: 0.8, //ボタンの枠の太さに対する＋,-の線の太さ
      num_btn_ratio_width: 0.75, //数字ボタンエリアの横幅に対する数字ボタンの横幅の割合
      num_btn_ratio_height: 0.5, //数字ボタンエリアの高さに対する数字ボタンの高さの割合
      font_ratio: 0.8, //数字ボタンの高さに対するフォントサイズの割合
      font_name: "meirio", //フォントの書体
      r: 10, //数字ボタンの枠の角の半径
    }
  },
  methods: {
    draw_plus_btn(plus_color){
      //＋ボタンの枠
      this.context.globalCompositeOperation = "destination-out";
      this.context.beginPath();
      this.context.moveTo(this.width-this.triangle_box_width*0.5, this.height*0.5-this.center_margin*0.5-this.triangle_height); 
      this.context.lineTo(this.width-this.triangle_box_width*0.5-this.triangle_width*0.5, this.height*0.5-this.center_margin*0.5); 
      this.context.lineTo(this.width-this.triangle_box_width*0.5+this.triangle_width*0.5, this.height*0.5-this.center_margin*0.5);
      this.context.closePath();	
      this.context.fillStyle = "rgba(0,0,0,1)";
      this.context.fill();
      this.context.globalCompositeOperation = "source-over";
      this.context.lineWidth = this.flame_weight;
      this.context.strokeStyle = "rgb(0,0,0)"; 
      this.context.stroke();
      this.context.fillStyle = plus_color;
      this.context.fill();

      //＋ボタンの"＋"の縦棒 
      this.context.strokeStyle = "rgb(0,0,0)"; 
      this.context.stroke();
      this.context.fillStyle="rgba(0,0,0,1)";
      this.context.fillRect(this.width-this.triangle_box_width*0.5-this.line_weight*0.5, this.height*0.5-this.center_margin*0.5-this.triangle_height*0.4-this.line_width*0.5, this.line_weight, this.line_width);

      //＋ボタンの"＋"の横棒
      this.context.strokeStyle = "rgb(0,0,0)"; 
      this.context.stroke();
      this.context.fillStyle="rgba(0,0,0,1)";
      this.context.fillRect(this.width-this.triangle_box_width*0.5-this.line_width*0.5,this.height*0.5-this.center_margin*0.5-this.triangle_height*0.4-this.line_weight*0.5, this.line_width, this.line_weight);
    },
    draw_minus_btn(minus_color){
      //-ボタンの枠
      this.context.globalCompositeOperation = "destination-out";
      this.context.beginPath();
      this.context.moveTo(this.width-this.triangle_box_width*0.5, this.height*0.5+this.center_margin*0.5+this.triangle_height); 
      this.context.lineTo(this.width-this.triangle_box_width*0.5-this.triangle_width*0.5, this.height*0.5+this.center_margin*0.5); 
      this.context.lineTo(this.width-this.triangle_box_width*0.5+this.triangle_width*0.5, this.height*0.5+this.center_margin*0.5);
      this.context.closePath();	
      this.context.fillStyle = "rgba(0,0,0,1)";
      this.context.fill();

      this.context.globalCompositeOperation = "source-over";
      this.context.lineWidth = this.flame_weight;
      this.context.strokeStyle = "rgb(0,0,0)"; 
      this.context.stroke();
      
      this.context.fillStyle = minus_color;
      this.context.fill();
   
      //-ボタンの"-"
      this.context.strokeStyle = "rgb(0,0,0)"; 
      this.context.stroke();
      this.context.fillStyle = "rgba(0,0,0,1)";
      this.context.fillRect(this.width-this.triangle_box_width*0.5-this.line_width*0.5, this.height*0.5+this.center_margin*0.5+this.triangle_height*0.4-this.line_weight*0.5, this.line_width, this.line_weight);
  
    },
    draw_1_btn(num_color){
      //数字ボタンの枠
      this.context.globalCompositeOperation = "destination-out";
      this.context.beginPath();
      this.context.moveTo(this.side_margin+this.r,this.top_margin);
      this.context.arc(this.side_margin+this.r, this.top_margin+this.r, this.r, Math.PI*1.5, Math.PI, true);  
      this.context.lineTo(this.side_margin, this.height-this.top_margin-this.r);
      this.context.arc(this.side_margin+this.r, this.height-this.top_margin-this.r, this.r, Math.PI, Math.PI*0.5, true); 
      this.context.lineTo(this.num_box_width-this.side_margin-this.r,this.height-this.top_margin);
      this.context.arc(this.num_box_width-this.side_margin-this.r, this.height-this.top_margin-this.r, this.r, Math.PI*0.5, Math.PI*0, true); 
      this.context.lineTo(this.num_box_width-this.side_margin,this.top_margin+this.r);
      this.context.arc(this.num_box_width-this.side_margin-this.r, this.top_margin+this.r, this.r, Math.PI*0, Math.PI*1.5, true); 
      this.context.closePath();
      this.context.fillStyle = "rgba(0,0,0,1)";
      this.context.fill();

      this.context.globalCompositeOperation = "source-over";
      this.context.fillStyle = num_color;
      this.context.fill();

      //数字ボタンの"数字"
      this.context.font = this.font;
      this.context.fillStyle = "rgba(0,0,0,1)";
      this.context.textAlign = "center";
      this.context.fillText(this.amount, this.num_box_width*0.5, this.top_margin+this.height*this.num_btn_ratio_height*0.8,this.num_box_width*this.num_btn_ratio_width);
    
      this.context.lineWidth = this.flame_weight;	
      this.context.strokeStyle = "rgb(0,0,0)"; 
      this.context.stroke();
    },
    click_res(plus_color,minus_color,num_color){
      let x = 0; //クリック押された場所のキャンバス中のx座標
      let y = 0; //クリック押された場所のキャンバス中のy座標
      let x2 = 0; //クリックが離された場所のキャンバス中のx座標
      let y2 = 0; //クリック離された場所のキャンバス中のy座標
      let num_down = false; //数字ボタンが押されているか
      let k1 = this.triangle_height/(this.triangle_width*0.5)*(-1); //＋ボタンの左斜辺の式の傾き
      let k2 = k1*(-1); //＋ボタンの右斜辺の式の傾き
      let k3 = k2; //-ボタンの左斜辺の式の傾き
      let k4 = k1; //-ボタンの右斜辺の式の傾き
      let b1 = this.height*0.5-this.center_margin*0.5-this.triangle_height - k1*(this.width-this.triangle_box_width*0.5); //＋ボタンの左斜辺の式の切片
      let b2 = this.height*0.5-this.center_margin*0.5-this.triangle_height - k2*(this.width-this.triangle_box_width*0.5); //＋ボタンの右斜辺の式の切片
      let b3 = this.height*0.5+this.center_margin*0.5+this.triangle_height - k3*(this.width-this.triangle_box_width*0.5); //―ボタンの左斜辺の式の切片
      let b4 = this.height*0.5+this.center_margin*0.5+this.triangle_height - k4*(this.width-this.triangle_box_width*0.5); //―ボタンの右斜辺の式の切片


      let d = this.$refs.canv;
      //クリックされた座標を取得
      d.onmousedown = (e) => {
        let rectdown = e.target.getBoundingClientRect();
        x = e.clientX - Math.floor(rectdown.left);
        y = e.clientY - Math.floor(rectdown.top);

        //+ボタン
        if(this.width-this.triangle_box_width*0.5-this.triangle_width*0.5<x && x<this.width-this.triangle_box_width*0.5+this.triangle_width*0.5 && this.height*0.5-this.center_margin*0.5-this.triangle_height<y && y<this.height*0.5-this.center_margin*0.5 && k1*x+b1<y && k2*x+b2<y) {      
          if(this.amount < this.max) {
            this.amount++;
            }
          else {
          }
          plus_color = "rgba(0,0,0,0.4)";
          this.draw_plus_btn(plus_color);

        }
        //―ボタン
        else if(this.width-this.triangle_box_width*0.5-this.triangle_width*0.5<x && x<this.width-this.triangle_box_width*0.5+this.triangle_width*0.5 && this.height*0.5+this.center_margin*0.5<y && y<this.height*0.5+this.center_margin*0.5+this.triangle_height && y<k3*x+b3 && y<k4*x+b4){
          if(this.amount >  this.min) {
            this.amount--;
          }
          else {
          }
          minus_color = "rgba(0,0,0,0.4)";
          this.draw_minus_btn(minus_color);         
        }
        //数字ボタン 
        else if(this.side_margin<x && x<this.num_box_width-this.side_margin && this.top_margin<y && y<this.height-this.top_margin) {
          //this.$emit('numbuttonclick', this.amount)
          let gradient6 = this.context.createLinearGradient(this.num_box_width*0.5,this.top_margin,this.num_box_width*0.5,this.height-this.top_margin);
          gradient6.addColorStop(0.0 , 'rgba(0,0,0,0.5)');
          gradient6.addColorStop(0.8 , 'rgba(0,0,0,0.3)');
          num_color = gradient6;
          this.draw_1_btn(num_color);
          
          return num_down = true;
        }
        //それ以外
        else {
        }
      }
      d.onmouseup = (e) => {
        let rectup = e.target.getBoundingClientRect();
        x2 = e.clientX - Math.floor(rectup.left);
        y2 = e.clientY - Math.floor(rectup.top);

        //+ボタン
        plus_color = this.gradient;
        this.draw_plus_btn(plus_color);
        //―ボタン
        minus_color = this.gradient2;
        this.draw_minus_btn(minus_color); 
        //数字ボタン
        num_color = this.gradient3;
        this.draw_1_btn(num_color);
        //数字ボタンが押されて且つ数字ボタン上で離されたとき
        if (num_down == true && this.side_margin<x2 && x2<this.num_box_width-this.side_margin && this.top_margin<y2 && y2<this.height-this.top_margin) {
          this.$emit('numbuttonclick', this.amount);        
        }
      }
      d.onmouseout = (e) => {
        //+ボタン
        plus_color = this.gradient;
        this.draw_plus_btn(plus_color);
        //―ボタン
        minus_color = this.gradient2;
        this.draw_minus_btn(minus_color); 
        //数字ボタン
        num_color = this.gradient3;
        this.draw_1_btn(num_color);
      }
    }
  },
  mounted() {
    this.amount = this.initial

    this.num_box_width = this.width*this.num_box_ratio; //数字ボタンのエリアの横幅

    this.triangle_box_ratio = 1-this.num_box_ratio; //三角ボタンのエリアの横幅の割合
    this.triangle_box_width = this.width*this.triangle_box_ratio; //三角ボタンのエリアの横幅
    
    this.triangle_width = this.triangle_box_width*this.triangle_ratio_width; //三角ボタンの横幅
    this.triangle_height = this.height*this.triangle_ratio_height; //三角ボタンの高さ
    this.center_margin = this.height*this.triangle_margin_ratio; //三角ボタン間の幅
    this.flame_weight = this.width*this.flame_weight_ratio //ボタンの枠の太さ

    
    this.line_width = this.triangle_height*this.line_ratio; //＋,-の線の長さ
    this.line_weight = this.flame_weight*this.line_weight_ratio; 

    
    let a = this.$refs.canv;
    this.context = a.getContext('2d');
    
    this.gradient0 = this.context.createLinearGradient(this.width*0.5, 0, this.width*0.5, this.height);
    this.gradient0.addColorStop(0.0 , 'rgba(0,50,255,0.05)');
    this.gradient0.addColorStop(0.8 , 'rgba(0,50,255,0.25)');
    this.context.fillStyle = this.gradient0;
    this.context.fillRect(0,0,this.width,this.height);
    
    //+ボタンを作成
    let plus_color; //＋ボタンの色
    this.gradient = this.context.createLinearGradient(this.width-this.triangle_box_width*0.5,this.height*0.5-this.center_margin*0.5-this.triangle_height,this.width-this.triangle_box_width*0.5,this.height*0.5-this.center_margin*0.5);
    this.gradient.addColorStop(0.0 , 'rgba(0,0,0,0.35)');
    this.gradient.addColorStop(0.8 , 'rgba(0,0,0,0.05)');
    plus_color = this.gradient;
    this.draw_plus_btn(plus_color);    

    //-ボタンを作成
    let minus_color; //-ボタンの色

    this.gradient2 = this.context.createLinearGradient(this.width-this.triangle_box_width*0.5, this.height*0.5+this.center_margin*0.5+this.triangle_height, this.width-this.triangle_box_width*0.5, this.height*0.5+this.center_margin*0.5);
    this.gradient2.addColorStop(0.0, 'rgba(0,0,0,0.35)');
    this.gradient2.addColorStop(0.8 , 'rgba(0,0,0,0.05)');
    minus_color = this.gradient2 
    this.draw_minus_btn(minus_color);
    
    //数字ボタンを作成
    
    this.side_margin = this.num_box_width*(1-this.num_btn_ratio_width)*0.5; //数字ボタンエリアと数字ボタンの間の横幅
    this.top_margin = this.height*(1-this.num_btn_ratio_height)*0.5; //数字ボタンエリアと数字ボタンの間の高さ
    
    this.font_size = this.height*this.num_btn_ratio_height*this.font_ratio; //フォントサイズ
    this.font = `${this.font_size}px ${this.font_name}`; //フォントサイズと書体    
    
    let num_color; //数字ボタンの色
    this.gradient3 = this.context.createLinearGradient(this.num_box_width*0.5,this.top_margin,this.num_box_width*0.5,this.height-this.top_margin);
    this.gradient3.addColorStop(0.0 , 'rgba(0,0,0,0.35)');
    this.gradient3.addColorStop(0.8 , 'rgba(0,0,0,0.05)');
    num_color = this.gradient3;
    this.draw_1_btn(num_color);
    
    //クリックされたら
    this.click_res(plus_color,minus_color,num_color);
  }
}
</script>

<style scoped>
</style>