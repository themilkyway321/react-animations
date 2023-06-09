# Framer Motion

React용 production-ready 모션 라이브러리 (오픈 소스)

https://www.framer.com/motion


Framer Motion

npm i framer-motion
```
import { motion } from "framer-motion"

const Box = styled(motion.div)`
  width: 200px;
  height: 200px;
  background-color: white;
  border-radius: 15px;
  box-shadow: 0 2px 3px rgba(0, 0, 0, 0.1), 0 10px 20px rgba(0, 0, 0, 0.06);
`;
const myVars = {
  start: { scale: 0 },
  end: { scale: 1, rotateZ: 360, transition: { type: "spring", delay: 0.5 } },
};
function App() {
  return (
    <Wrapper>
      <motion.div></motion.div>
      <Box
        transition={{ type: "spring", delay: 0.5 }}
        initial={{ scale: 0 }}
        animate={{ scale: 1, rotateZ: 360 }}
      />
      <Box variants={myVars} initial="start" animate="end" />
    </Wrapper>
  );
}
export default App;
```

## Animation

간단한 애니메이션의 경우 animate props에서 직접 값을 설정할 수 있다.

```
<motion.div animate={{ rotate: 360 }} transition={{ duration: 2 }}></motion.div>
```
https://www.framer.com/docs/animation


### 1. initial

initial: boolean | Target | VariantLabels (애니메이션의 초기값 지정)
속성, 변형 레이블 또는 시작할 변형 레이블의 배열입니다.
animate의 값으로 초기화하려면 false로 설정합니다(마운트 애니메이션 비활성화).
https://www.framer.com/docs/component/###initial

### 2. Transition
Transition은 값이 한 상태에서 다른 상태로 움직이는 방식을 정의합니다.
또한 Tween, Spring 또는 Inertia를 사용할 애니메이션 유형을 정의하는 소품을 허용할 수 있습니다.
ex) motion.div animate={{ rotate: 180 }} transition={{ type: 'spring' }}
https://www.framer.com/docs/transition


### 3. Variants

Variants은 컴포넌트가 가질 수 있는 미리 정의된 시각적 state입니다.
```
const myVars = {
  start: { scale: 0 },
  end: { scale: 1, rotateZ: 360, transition: { type: "spring", delay: 0.5 } },
};
function App() {
  return (
    <Wrapper>
      <Box variants={myVars} initial="start" animate="end" />
    </Wrapper>
  );
}
export default App;
```
https://www.framer.com/docs/introduction/##variants



### 4. Orchestration

delayChildren: 딜레이 시간(초) 후에 하위 애니메이션이 시작됩니다.
staggerChildren: 하위 컴포넌트의 애니메이션에 지속 시간(초)만큼 시차를 둘 수 있습니다. 예를 들어, staggerChildren이 0.01이면 첫 번째 자식은 0초, 두 번째 자식은 0.01초, 세 번째 자식은 0.02초 지연되는 식입니다. 계산된 stagger 딜레이가 delayChildren에 추가됩니다.


https://www.framer.com/docs/component/###inherit

boxVars 에 줬던 start와 end속성이 자식인 Circle에도 상속된다. 
```
const boxVars = {
  start:{
    opacity:0,
    scale:0.5,
  },
  end: {
    scale:1,
    opacity:1,
    transition:{
      type:"spring",
      duration:0.5,
      bounce:0.5,
      delayChildren: 0.5,
      staggerChildren: 0.2,
    },
  },
};
const circleVariants = {
  start:{
    opacity:0,
    x:0,
    y: 1000,
  },
  end:{
      opacity:1, 
      x:0,
      y: 0,
    },
}

function App() {
  return (
    <Wrapper>     
      <Box variants={boxVars} initial="start" animate="end" >
        <Circle variants={circleVariants} />
        <Circle variants={circleVariants} />
        <Circle variants={circleVariants} />
        <Circle variants={circleVariants} />
      </Box>
    </Wrapper>
  );
}
```



### 5. while~




- whileHover: VariantLabels | TargetAndTransition

  호버 제스처가 인식되는 동안 애니메이션할 속성 또는 변형 레이블입니다.

  < motion.div whileHover={{ scale: 0.8 }} / >
  https://www.framer.com/docs/gestures/#hover

- whileTap: VariantLabels | TargetAndTransition

컴포넌트를 누르고 있는 동안 애니메이션할 속성 또는 변형 레이블입니다.

< motion.div whileTap={{ scale: 0.8 }} / >

https://www.framer.com/docs/gestures/#tap

### 6. Drag

  drag: boolean | "x" | "y"

  이 요소에 대해 끌기를 활성화합니다. 기본적으로 false로 설정됩니다. 양방향으로 드래그하려면 true로 설정하십시오. 특정 방향으로만 드래그하려면 "x" 또는 "y"를 설정합니다.
  < motion.div drag="x" / >

  whileDrag: VariantLabels | TargetAndTransition
드래그 제스처가 인식되는 동안 애니메이션할 속성 또는 변형 레이블입니다.

< motion.div whileDrag={{ scale: 1.2 }} / >

https://www.framer.com/docs/gestures/#drag

  whileDrag 에 배경색을 rgb컬러로 주면, motion에서 알아서  rbg숫자가 점점 변해가는 transition 효과를 준다.


  ```
  const boxVars = {
  hover:{scale:1.5, rotateZ:90},
  click:{scale:1,borderRadius:"100px"},
  drag:{backgroundColor:"rgb(52, 152, 219)", transition:{duration:10}},
};
const circleVariants = {
  
}

function App() {
  return (
    <Wrapper>     
      <Box drag variants={boxVars} whileDrag="drag" whileHover="hover" whileTap="click" />
    </Wrapper>
  );
}
export default App;
```


- dragConstraints

허용된 드래그 가능 영역에 제약 조건을 적용합니다.
dragConstraints 에는 드래그 가능한 컴포넌트의 가장자리 거리를 정의합니다. (드래그 가능한 영역에 가장자리에서 얼마만큼까지 허용할 것인지 지정)
```
// 픽셀 이용
< motion.div drag="x" dragConstraints={{ left: 0, right: 300 }}/ >

// ref이용
function App() {
  const biggerBoxRef = useRef<HTMLDivElement>(null);
  return (
    <Wrapper>     
    <BiggerBox ref={biggerBoxRef}>
      <Box 
      drag 
      dragSnapToOrigin
      dragElastic={0.5}
      dragConstraints={biggerBoxRef} 
      variants={boxVars} whileDrag="drag" whileHover="hover" whileTap="click" />
      </BiggerBox>
    </Wrapper>
  );
}
```

dragSnapToOrigin: boolean
true인 경우 드래그 가능한 요소는 드래그를 놓을 때, 원점으로 다시 애니메이션됩니다.
dragSnapToOrigin={true}

dragElastic: DragElastic
외부 제약 조건에서 허용되는 이동 정도. 0 = 움직임 없음, 1 = 전체 움직임. 기본적으로 0.5로 설정됩니다. 움직임을 비활성화하기 위해 false로 설정할 수도 있습니다.
dragElastic={0.2}

https://www.framer.com/docs/gestures/#drag


### 7. MotionValue

MotionValues는 애니메이션 값의 상태(state)와 속도(velocity)를 추적합니다. 모든 모션 컴포넌트는 내부적으로 MotionValues를 사용하여 애니메이션 값의 상태와 속도를 추적합니다. 일반적으로 이들은 자동으로 생성됩니다. (MotionValue는 React State가 아니기 때문에 Motion Value값이 바뀌어도 리랜더링이 일어나지 않는다.)

```
import { motion, useMotionValue } from "framer-motion"

function App() {
 const x = useMotionValue(0);
useMotionValueEvent(x, "change", (l) => {
  console.log(l);
  });
  return (
    <Wrapper>
    <button onClick={()=>x.set(200)}>Click me</button>
      <Box style={{x}} drag="x" dragSnapToOrigin />
    </Wrapper>
  );
}
export default App;
```
const x = useMotionValue(0)
useMotionValue 후크로 MotionValues를 생성할 수 있습니다. useMotionValue에 전달된 값은 MotionValue의 초기 상태로 작동합니다.

x.set(100)
set 메서드로 업데이트할 수 있습니다.
이것은 React 리렌더링을 트리거하지 않습니다.

### 7-2. MotionValue

useTransform

useTransform 훅을 통해 MotionValues를 연결합니다.
useTransform()는 한 값 범위에서 다른 값 범위로 매핑하여 다른 MotionValue의 output을 변환하는 MotionValue를 만듭니다.
x(Motion Value)값을 다른 output값으로 변환해준다.
ex) x: -400 => 1

https://www.framer.com/docs/motionvalue/##usetransform


potato가 x값의 인풋값을 다른 아웃풋 값으로 변경해준다. 

x의 위치가 -800 일때 scale을 2로 변경
x의 위치가 0일때 scale을 1로 변경
x의 위치가 800일때 scale을 0.1로 변경 


```
function App() {
 const x = useMotionValue(0);
 const potato = useTransform(x, [-800, 0, 800], [2, 1, 0.1]);
 
useMotionValueEvent(potato, "change", (l) => {
  console.log(l);
  }); 
  return (
    <Wrapper>
      
      <Box style={{x, scale:potato}} drag="x" dragSnapToOrigin />
    </Wrapper>
  );
}
```

potato는 임의의 이름으로 다른 것으로 바꿔도 된다. scale로 변경하게 되면 코드가 축약된다. 
```
function App() {
 const x = useMotionValue(0);
 const scale = useTransform(x, [-800, 0, 800], [2, 1, 0.1]);
 
useMotionValueEvent(scale, "change", (l) => {
  console.log(l);
  }); 
  return (
    <Wrapper>
      
      <Box style={{x, scale}} drag="x" dragSnapToOrigin />
    </Wrapper>
  );
}
```


### 8. useScroll

뷰포트가 스크롤될 때 업데이트되는 MotionValues를 리턴합니다.

아래 값들은 모두 MotionValue< number >를 넘겨줍니다.
- scrollX: 실제 수평 스크롤 픽셀 ex) 500px
- scrollY: 실제 수직 스크롤 픽셀 ex) 500px
- scrollXProgress : 0 ~ 1 사이의 수평 스크롤
- scrollYProgress : 0 ~ 1 사이의 수직 스크롤(가장 상단 0, 가장 하단 1)


스크롤하여 가장 하단 0이 될때, Box scale은 1이되고
스크롤하여 가장 상단 1이 될때, box scale이 5가 되는 코드 

```
function App() {
 const x = useMotionValue(0);
 const rotateZ = useTransform(x, [-800, 800], [-360,360]);
 const gradient = useTransform(x,[-800,800],
  [
  "linear-gradient(135deg, rgb(0, 210, 238), rgb(0, 83, 238))",
  "linear-gradient(135deg, rgb(0, 238, 155), rgb(238, 178, 0))",
  ]
  );
 const {scrollYProgress}=useScroll();
const scale = useTransform(scrollYProgress,[0,1],[1,5])
  return (
    <Wrapper style={{background:gradient}}>
      <Box style={{x, rotateZ, scale}} drag="x" dragSnapToOrigin />
    </Wrapper>
  );
}
```


### 9. pathLength

svg 엘리먼트에 'pathLength', 'pathSpacing', 'pathOffset' 속성을 사용하여 Line drawing 애니메이션을 만들 수 있습니다.
https://www.framer.com/docs/examples/#line-drawing


아래코드 에서 transition을 다음과 같이 주면, 특정 애니메이션에 delay와 duration을 줄 수 있다. 

 transition={{
        default:{duration:5},
        fill:{duration:1, delay:3}
      }}

```
const Svg = styled.svg`
width:300px;
height: 300px;

`;

const svgVarient = {
  start:{
    pathLength:0,
    fill:"rgba(255,255,255,0)"
    },
  end:{pathLength:1,
    fill:"rgba(255,255,255,1)",
    
  },
};
function App() {
 const x = useMotionValue(0);
 const rotateZ = useTransform(x, [-800, 800], [-360,360]);
 const gradient = useTransform(x,[-800,800],
  [
  "linear-gradient(135deg, rgb(0, 210, 238), rgb(0, 83, 238))",
  "linear-gradient(135deg, rgb(0, 238, 155), rgb(238, 178, 0))",
  ]
  );
 const {scrollYProgress}=useScroll();
const scale = useTransform(scrollYProgress,[0,1],[1,5])
  return (
    <Wrapper>
     <Svg 
     xmlns="http://www.w3.org/2000/svg" 
     height="1em" 
     viewBox="0 0 448 512">
      <motion.path 
      strokeWidth="2"
      stroke="white"
      variants={svgVarient}
      initial="start"
      animate="end"
      transition={{
        default:{duration:5},
        fill:{duration:1, delay:3}
      }}
      d="M224 373.12c-25.24-31.67-40.08-59.43-45-83.18-22.55-88 112.61-88 90.06 0-5.45 24.25-20.29 52-45 83.18zm138.15 73.23c-42.06 18.31-83.67-10.88-119.3-50.47 103.9-130.07 46.11-200-18.85-200-54.92 0-85.16 46.51-73.28 100.5 6.93 29.19 25.23 62.39 54.43 99.5-32.53 36.05-60.55 52.69-85.15 54.92-50 7.43-89.11-41.06-71.3-91.09 15.1-39.16 111.72-231.18 115.87-241.56 15.75-30.07 25.56-57.4 59.38-57.4 32.34 0 43.4 25.94 60.37 59.87 36 70.62 89.35 177.48 114.84 239.09 13.17 33.07-1.37 71.29-37.01 86.64zm47-136.12C280.27 35.93 273.13 32 224 32c-45.52 0-64.87 31.67-84.66 72.79C33.18 317.1 22.89 347.19 22 349.81-3.22 419.14 48.74 480 111.63 480c21.71 0 60.61-6.06 112.37-62.4 58.68 63.78 101.26 62.4 112.37 62.4 62.89.05 114.85-60.86 89.61-130.19.02-3.89-16.82-38.9-16.82-39.58z"/></Svg>
    </Wrapper>
  );
}
```



### 10. AnimatePresence

AnimatePresence를 사용하면 React 트리에서 컴포넌트가 제거될 때 제거되는 컴포넌트에 애니메이션 효과를 줄 수 있습니다. React에는 다음과 같은 수명 주기 메서드가 없기 때문에 종료 애니메이션을 활성화해야 합니다.

exit
이 컴포넌트가 트리에서 제거될 때 애니메이션할 대상입니다.
```
import { motion, AnimatePresence } from "framer-motion"

const boxVars = {
  start:{
    opacity:0,
    scale:0,
  },
  visible:{
    opacity:1,
    scale:1,
    rotateZ:360,
  },
  leaving:{
    opacity:0,
    scale:0,
    y:50,
  },
};

function App() {
 const [showing, setShowing] = useState(false);
 const toggleShowing =()=>setShowing((prev)=>!prev);
  return (
    <Wrapper>
     <button onClick={toggleShowing}>Click me!</button>
    <AnimatePresence> {showing? <Box variants={boxVars} initial="start" animate="visible" exit="leaving" />: null}</AnimatePresence>
    </Wrapper>
  );
}
```
https://www.framer.com/docs/animate-presence/



AnimatePresence의 단일 자식 key를 변경하여 슬라이드쇼(슬라이더)와 같은 컴포넌트를 쉽게 만들 수 있습니다.

https://www.framer.com/docs/animate-presence/##unmount-animations

Slider 예시 코드
https://codesandbox.io/s/framer-motion-image-gallery-pqvx3?from-embed


아래 코드 리뷰

1단계:  const [num, setNum]= useState(1); 으로 숫자를 변경하면서 박스 생성

2단계: box에 애니메이션 속성을 주고 

3단계:  const [back, setBack] = useState(false); 을 만들어서, back이 true냐 false에 따라 x축을 다르게 조정 

```
const box = {
  entry:(back:boolean) =>({
      x:back? -500:500,
      opacity:0,
      scale:0,
  }),
  center:(back:boolean) =>({
    x:0,
    opacity:1,
    scale:1,
    transition:{duration:0.3}
    
  }),
  exit:(back:boolean) =>({
    x:back? 500:-500,
    opacity:0,
    scale:0,
    transition:{duration:0.3}
  }),
};


function App() {
  const [num, setNum]= useState(1);
  const [back, setBack] = useState(false);
  const prevPlease = ()=> {
    setBack(true);
    setNum((prev) =>(prev === 1 ? 1: prev - 1))
  };
  const nextPlease = ()=>{ 
    setBack(false);
    setNum((prev) =>(prev === 10 ? 10: prev + 1))
  };
  return (
    <Wrapper>
    <AnimatePresence mode="wait" custom={back}>     
       <Box 
       custom={back}
       variants={box} 
       initial="entry" 
       animate="center" 
       exit="exit" 
       key={num}>{num}</Box>
    </AnimatePresence>
    <button onClick={prevPlease}>prev</button>
    <button onClick={nextPlease}>next</button>
    </Wrapper>
  );
}
export default App;
```

### 11. Layout animation

layout: boolean | "position" | "size"
true인 경우 이 컴포넌트는 레이아웃이 변경될 때 새 위치에 자동으로 애니메이션을 적용합니다. 크기나 위치가 변경될 때 모션 컴포넌트의 레이아웃에 자동으로 애니메이션을 적용하려면 레이아웃 prop을 제공합니다. 부모 플렉스박스 방향, 너비, 상단/오른쪽 등 레이아웃 변경의 원인이 무엇이든 상관없이 애니메이션 자체는 최대 성능을 위해 변환으로 수행됩니다.
ex) < motion.div layout>< /motion.div>

Syncing layout animations
모션 컴포넌트의 layout prop은 레이아웃이 변할 때마다, 자동으로 애니메이션을 적용합니다.
https://www.framer.com/docs/animate-shared-layout/#syncing-layout-animations

Animate between components
AnimateSharedLayout은 동일한 layoutId prop을 가진 모션 컴포넌트들 간에 애니메이션을 적용할 수 있습니다. layoutId가 있는 새 컴포넌트가 추가되고 다른 컴포넌트가 제거되면 이전 컴포넌트에서 새 컴포넌트로 레이아웃 애니메이션을 수행합니다. 새 컴포넌트는 이전 컴포넌트의 애니메이션 값도 초기 상태로 상속합니다. 따라서 시각적으로 하나의 연속 컴포넌트로 처리됩니다.
ex) isSelected && < motion.div layoutId="underline" />
https://www.framer.com/docs/animate-shared-layout/#animate-between-components



```
function App() {
  const [clicked, setClicked] = useState(false);
  const toggleClicked = ()=> setClicked((prev)=>!prev);
  return (
    <Wrapper onClick={toggleClicked}>
    <Box>{!clicked? <Circle layoutId="circle" style={{borderRadius:50, scale:0.5}} /> : null}</Box>
    <Box>{!clicked? null : <Circle layoutId="circle" style={{borderRadius:0, scale:1}} />}</Box>
    </Wrapper>
  );
}
```