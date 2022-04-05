# Canvas로 화살표 그리기 with React

## Canvas 라는 Component 생성

```
import { mockData } from 'assets/mockData'; // mockData는 좌표 정보가 들어 있다
import storeImg from 'assets/store-sample-img.png'; // background image를 불러온다
import { drawArrow } from 'modules/drawArrow'; // 화살표를 canvas에 그리는 함수
import { useEffect, useRef, useState } from 'react';

const Canvas = () => {
  const canvasRef = useRef<HTMLCanvasElement>(null); // React에서 Dom에 접근하기 위한 방법으로 useRef를 사용하는데 해당 tag에 useRef를 속성값으로 넣어주면 useRef.currnet 객체에 담기게 된다
  const store = mockData.store;

  const [canvasSize, setCanvasSize] = useState({ width: 0, height: 0 });

  useEffect(() => {
    const canvas = canvasRef.current;

    if (!canvas) return; // useRef 객체가 Dom에 접근한 경우에만 진행
    const ctx = canvas.getContext('2d');

    if (!ctx) return;
    const img = new Image();
    img.onload = () => {
      const { width, height } = img;
      setCanvasSize({ width, height }); // image 사이즈 상태 관리
      ctx.drawImage(img, 0, 0); // 해당 image를 [0,0]에서 시작된다는 의미
      store.forEach((v, i) => { // 이중 forEach문을 통하여 좌표 당 2개의 화살표 표출
        store.forEach((w, j) => {
          if (i <= j) return; // 첫번째 화살표와 두번째 화살표의 중복을 방지
          drawArrow( storeImg, ctx, v.coodinate[0], v.coodinate[1], w.coodinate[0],
                      w.coodinate[1], 4, 8, false, true, 2, true
                    ); // drawArrow의 필요 인수 = (img,canvas.getContext,좌표1x,좌표1y,
                                                좌표2x,좌표2y,화살촉 길이,화살촉넓이,시작점 화살표 유무, 끝점 화살표 유무,선 굵기,두번째 화살표인지 확인)
          drawArrow( storeImg, ctx, w.coodinate[0], w.coodinate[1], v.coodinate[0],
                      v.coodinate[1], 4, 8, false, true, 2, false
                    );
        });
      });
    };
    img.src = storeImg; // import한 image 파일을 img tag의 src로 설정
                           이 설정이 로직의 가장 처음으로 진행되어야 img.onload 함수가 실행 가능
  }, [store]);

  return (
    <>
      <canvas ref={canvasRef} width={canvasSize.width} height={canvasSize.height}></canvas> // canvas의 넓이와 높이는 필수 입력 필요
    </>
  );
};

export default Canvas;
```

## drawArrow 함수 모듈(곡선 화살표로 표출)

```
export function drawArrow(
  storeImg: string,
  ctx: CanvasRenderingContext2D,
  x0: number, // 시작점 x
  y0: number, // 시작점 y
  x1: number, // 끝점 x
  y1: number, // 끝점 y
  aWidth: number, // 화살촉이 선에서 수직으로 연장되는 거리
  aLength: number, // 화살 날개의 길이
  arrowStart: boolean, // 시작점에 화살촉 유무 결정
  arrowEnd: boolean, // 끝점에 화살촉 유무 결정
  lineWidth: number, // 선의 전체 굵기
  secondary: boolean
) {
  const dx = x1 - x0;
  const dy = y1 - y0;
  const angle = Math.atan2(dy, dx);
  const length = Math.sqrt(dx * dx + dy * dy);
  const arrowWholeLength = 30; // 좌표와 좌표 사이의 화살표 전체 길이 조절
  const arcLength = 50; // 화살표의 곡선 각도 조절

  if (ctx) {
    ctx.lineWidth = lineWidth; // 선 굵기 지정
    ctx.strokeStyle = 'blue'; // 선 색 지정
    ctx.translate(x0, y0); // 원점 기준 (x0, y0) 좌표로 이동
    ctx.rotate(angle); // canvas가 이동된 후 canvas 자체를 angle각도 만큼 회전

    ctx.beginPath(); // 하위 경로 초기화, 새로 그리기
    if (!secondary) { // 두번째 화살표가 아니면 진행
      ctx.moveTo(arrowWholeLength, 0); // 화살표 시작점 설정
      ctx.bezierCurveTo( // canvas 곡선 만들기 참조
        arrowWholeLength,
        arcLength,
        length - arrowWholeLength,
        arcLength,
        length - arrowWholeLength,
        0
      );
      if (arrowEnd) {
        ctx.moveTo(length - arrowWholeLength - aLength + 10, -aWidth + 10);
        ctx.lineTo(length - arrowWholeLength, 0);
        ctx.lineTo(length - arrowWholeLength - aLength + 2, aWidth);
        ctx.closePath();
      }
    } else {
      ctx.strokeStyle = 'white';
      ctx.moveTo(arrowWholeLength, 0);
      ctx.bezierCurveTo(
        arrowWholeLength,
        arcLength,
        length - arrowWholeLength,
        arcLength,
        length - arrowWholeLength,
        0
      );

      if (arrowEnd) {
        ctx.moveTo(length - arrowWholeLength - aLength + 10, -aWidth + 10); 화살촉 시작점 설정
        ctx.lineTo(length - arrowWholeLength, 0); // 화살촉 좌측 설정
        ctx.lineTo(length - arrowWholeLength - aLength + 2, aWidth); // 화살촉 우측 설정
        ctx.closePath(); // 설정된 좌표들을 선으로 연결
      }
    }
    ctx.stroke(); // canvas에 그리는 명령
    ctx.setTransform(1, 0, 0, 1, 0, 0); // 위의 과정 이후 다음 작업은 기존 그림과 별개로
                                           새로 그린다는 의미
  }
}

```

<img src="./../Image/arrow%20on%20canvas.png" width="700px" height="550px" alt=""></img>
