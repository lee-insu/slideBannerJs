# slideBannerJs
vanilla js로 구현한 슬라이드바  


css

    #slide {
        position: relative;
        overflow: hidden;
        width: 1800px;
        height: 300px;
    }
    
    #slide .slide-list>li {
        position: absolute;
        top: 0;
        left: 1800px;
        width: 1800px;
        height: 300px;
        text-align: center;
        font-size: 30px;
        line-height: 300px;
        color: #fff;
    }
    
    #slide .slide-list>li:nth-child(1) {
        background-color: red;
        left: 0;
    }
    
    #slide .slide-list>li:nth-child(2) {
        background-color: orange;
    }
    
    #slide .slide-list>li:nth-child(3) {
        background-color: green;
    }
    
    #slide .slide-list>li:nth-child(4) {
        background-color: blue;
    }
    
    #slide .btn>button {
        position: absolute;
        top: 50%;
        transform: translateY(-50%);
        border: 0;
        padding: 5px;
        background-color: #fff;
    }
    
    #slide .btn .prev {
        left: 5px;
    }
    
    #slide .btn .next {
        right: 5px;
    }
  
    #slide .btn .prev {
      left: 5px;
    }
  
    #slide .btn .next {
      right: 5px;
    }
    
    #slide .auto>button {
        display: none;
        position: absolute;
        bottom: 5px;
        right: 5px;
        border: 0;
        padding: 5px;
        background-color: #fff;
    }
    
    #slide .bar {
        position: absolute;
        bottom: 10px;
        left: 50%;
        transform: translateX(-50%)
    }
    
    #slide .bar:after {
        content: '';
        display: block;
        clear: both;
    }
    
    #slide .bar>li {
        float: left;
        margin-left: 5px;
        border-radius: 50%;
        width: 12px;
        height: 12px;
        cursor: pointer;
        opacity: .3;
        background-color: #fff;
        list-style-type: none;
    }
  
    
    
    #slide .bar>li.on {
        opacity: 1
    }
    
    #slide .bar>li:first-child {
        margin-left: 0
    }



html


    <div id="slide">
        <ul class="slide-list">
            <li>1</li>
            <li>2</li>
            <li>3</li>
            <li>4</li>
        </ul>
        <div class="btn">
            <button type="button" class="prev">prev</button>
            <button type="button" class="next">next</button>
        </div>
        <div class="auto">
          <button type="button" class="stop">stop</button>
          <button type="button" class="play">play</button>
      </div>
    </div>


javascript


    window.addEventListener('load', function() {

     let MOVEING_PX = 6,
        AUTO_TIME = 2000,
        slide = document.getElementById('slide'),
        bar = document.createElement('ul'),
        slideList = slide.getElementsByClassName('slide-list'),
        slideCard = slideList[0].getElementsByTagName('li'),
        prevBtn = slide.getElementsByClassName('prev'),
        nextBtn = slide.getElementsByClassName('next'),
        playBtn = slide.getElementsByClassName('play'),
        stopBtn = slide.getElementsByClassName('stop'),
        playSet  = null,
        before = 0,
        after = 0,
        moveIng = false ;

        //li 갯수 만큼 bar의 li 생성
        for(let i = 0; i < slideCard.length; i++) {
            bar.innerHTML += '<li></li>';
          };

        //ul의 class name을 bar, li엔 class = 'on', slide 태그 안에 bar 생성 
         bar.classList.add('bar');
         bar.children[0].classList.add('on');
         slide.append(bar); 

        //bar의 자식 갯수 barclick 연결
        for(let j = 0; j <bar.children.length; j++) {
          barClick(j);
        };

        //null에 playCard, AUTO_TIME 반복, stop 버튼 생성
        playSet = setInterval(playCard,AUTO_TIME);
        stopBtn[0].style.display = 'block';



        //nextBtnSlide와 같지만 인자를 넘기는 차이와 넥스트 동시 입력시 오류로 따로 생성
      function playCard() {
        if(!moveIng) {
          //moveing이 true일 때 after 1씩 증가
          after ++;
          //li 개수보다 값이 많을 때 after 초기화(처음부터 시작)
          if(after >= slideCard.length) {
            after = 0;
          };
          //nextX, prevX, set
          move(after,before, 'next');
          before = after;
        };
      };


      function nextBtnSlide(event) {
        if(!moveIng) {
          after ++;
          if(after >= slideCard.length) {
              after = 0;
          };
          move(after,before,'next');
          before = after;
        };
      };

      function prevBtnSlide(event) {
        if(!moveIng) {
          //moveing이 true일 때 after 1씩 증가
          after --;
          //처음에서 마지막 prev로 넘어갈 때 
          if(after < 0) {
            after = slideCard.length -1;
          };
          move(after,before);
          before = after;
          
        };
      };


      function playBtnSlide(event) {
          playBtn[0].style.display = 'none';
          stopBtn[0].style.display = 'block';
          playSet = setInterval(playCard,AUTO_TIME);
      };

      function stopBtnSlide(event) {
        playBtn[0].style.display = 'block';
        stopBtn[0].style.display = 'none';
        clearInterval(playSet);
      };


      function barClick(event) {
        bar.children[event].addEventListener('click',function(){
          if(!moveIng) {
            after = event;
            if(after > before) {
                move(after, before, 'next');
            }else if(after < before) {
                move(after, before);
            };
            before = after;
          }
        });
      }


      nextBtn[0].addEventListener('click',nextBtnSlide);
      prevBtn[0].addEventListener('click',prevBtnSlide);
      playBtn[0].addEventListener('click',playBtnSlide);
      stopBtn[0].addEventListener('click',stopBtnSlide);


      function move(after,before, type) {
        //nextX가 type이 next일 때  slide.offsetWidth (next), 아닐 때 slide.offsetWidth * -1 (prev)
        let nextX = type === 'next' ? slide.offsetWidth : slide.offsetWidth * -1,
            prevX = 0,
            set = null;
            //'next'
            set = setInterval(moveEvent);

            function moveEvent() {
              moveIng =true;
              if(type === 'next') {
                nextX -= MOVEING_PX;
                slideCard[after].style.left = Number(nextX) + 'px';
                if(nextX <= 0) {
                  clearInterval(set);
                  nextX = slide.offsetWidth;
                  moveIng = false;
                };
                prevX -= MOVEING_PX;
              }else{
                nextX += MOVEING_PX;
                slideCard[after].style.left = Number(nextX) + 'px';
                if(nextX >= 0) {
                  clearInterval(set);
                  nextX = slide.offsetWidth * -1;
                  moveIng = false;
                };
                prevX += MOVEING_PX;
              };
              slideCard[before].style.left = Number(prevX) + 'px';

            };
            bar.children[before].classList.remove('on');
            bar.children[after].classList.add('on');

      };

    });

