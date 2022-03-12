
//add image //step3
function loadimage(){
	virus_image=new Image();
	virus_image.src="https://raw.githubusercontent.com/prateek27/game-webinar-javascript/d47c817d21c553384094b3230fa09db93b42c730/CoronaGame/Assets/v1.png"
    player_image=new Image();
    player_image.src="https://raw.githubusercontent.com/prateek27/game-webinar-javascript/d47c817d21c553384094b3230fa09db93b42c730/CoronaGame/Assets/superhero.png"
    gem_image =new Image();
    gem_image.src="https://raw.githubusercontent.com/prateek27/game-webinar-javascript/d47c817d21c553384094b3230fa09db93b42c730/CoronaGame/Assets/gem.png"
}

//try to work with canvas

function init(){ //step1
	canvas=document.getElementById("mycanvas");
    console.log(canvas);

    //height and width of canvas using javascript
    w=700;
    h=400;
    canvas.width=w;
    canvas.height=h;
    //try to work with canvas
    pen=canvas.getContext("2d");
    console.log(pen);
  score =0;
  game_over=false;


//create a bog using json objects
//enemy objects
e1={
  x:150,
  y:50,
  w:60,
  h:60,
  speed:20,
};
e2={
  x:300,
  y:150,
  w:60,
  h:60,
  speed:25,
};
e3={
  x:450,
  y:20,
  w:60,
  h:60,
  speed:30,
};
enemy=[e1,e2,e3];

//create player object
player={
  x:20,
  y:h/2,
  w:60,
  h:60,
  speed:20,
  move: "false",
}

//create gem object
gem={
  x:w-100,
  y:h/2,
  w:60,
  h:60,
}

  //create event listner for movement of player through key board 
    canvas.addEventListener('mousedown',function(){
		console.log("You pressed the mouse");
		player.moving = true;
	});
	canvas.addEventListener('mouseup',function(){
		console.log("You released the mouse");
		player.moving = false;
	});



}            
//add movement to bird  //step2
function draw(){ //initial values and add new images
     //clear the old screen rect area
     pen.clearRect(0,0,w,h);
   pen.fillStyle="red"; //box color changes to red
   //pen.fillRect(bird.x,bird.y,bird.w,bird.h); //for rectangle box
   pen.drawImage(player_image,player.x,player.y,player.w,player.h);
   //gem image
    pen.drawImage(gem_image,gem.x,gem.y,gem.w,gem.h);

    for(let i=0;i<enemy.length;i++){
     pen.drawImage(virus_image,enemy[i].x,enemy[i].y,enemy[i].w,enemy[i].h);
    }
    //score
    pen.fillStyle="white";
    pen.fillText("score :" +score,10,10);
}


// collision function between gem &player
function collidal(b1,b2){
    if(Math.abs(b1.x-b2.x)<=30 && Math.abs(b1.y-b2.y)<=30){
    	return true;
    }
    else{
    	return false;
    }
}
function update(){  //update the movement of virus
	if(player.moving==true){
		player.x+=player.speed;
		score+=20;
	}
	//collision between virus and player
  for(let i=0;i<enemy.length;i++){
  	if(collidal(enemy[i],player)){
  		score-=i*100;
			if(score<0){
				game_over = true;
				alert("Game Over");
			}
  	}
  }

	// check collidal
	if(collidal(gem,player)){
		game_over=true;
		draw();
		alert("your score:" +score);
		alert("you won");
		//break game
	}
	for(let i=0;i<enemy.length;i++){
        enemy[i].y+=enemy[i].speed;
       if(enemy[i].y>h-enemy[i].h || enemy[i].y<0){
       	enemy[i].speed =enemy[i].speed * (-1);
	   }
    }
     
}
//game loop will call draw and update function continuously
function gameloop(){
	if(game_over==true){
		clearInterval(f);
	}
	draw();
	update();
}

loadimage();
init(); //canvas size changes
//repeatedy call game loop
var f=setInterval(gameloop,100);

