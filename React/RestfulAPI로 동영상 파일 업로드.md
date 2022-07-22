# RestfulAPI로 동영상 파일 업로드

## 1. 인터페이스

| **기능**      | **Type** | **Url**            | **Request**                                 | **Response** |
| ------------- | -------- | ------------------ | ------------------------------------------- | ------------ |
| 동영상 업로드 | POST     | /test-video-upload | # 동영상 </br> Content-type </br> key: file | response     |

## 2. Api 요청 코드 (axios)

```
src/api // api module
const uploadVideo = async() => {
 const ret = [undefined, undefined];
 try{
 const response = await axios.post('http://localhost:3000/test-video-upload', body?, {
        withCredentials: false,    // CORS 처리를 위한 false
        headers: {
          'Content-Type': 'multipart/form-data',
        },
      });
      ret[0] = response.data;
 } catch (err) {
      ret[1]
 }
 return ret;
}
```

- post 요청 시, url, body, headers를 요청하고 인터페이스에서 Content-type를 같이 보내달라는 요구에 따라 headers에 넣어준다
- 동영상 파일 같은 경우 Content-Type으로 multipart/form-data라고 설정한다.

```
src/UploadVideo // upload video component

  const VideouploadHandler = async(file) {
    const [res, err] = await uploadVideo(file);

    if (res?.res_info.result === 'success') console.log('success upload video');
    else if (err) console.log('Error Message :', err);
    else if (res?.res_info.result === 'failed')
      console.log('Error Message :', res.res_info.message);
  }
```

- 해당 api를 사용하는 component에서 불러와 file을 전달하면서 사용

## 3. file 전달 방법

- 동영상 파일을 전달하는 방법은 FormData에 담아 전달하는 방법이 있다.
- FormData에 파일을 저장하는 방법은 3가지 정도 시도하였다.

  ### a. formData.append

  ```
  const formData = new FormData();

    const handleChangeFileName = (e) => {
    const { files } = e.target;

    if (files && files.length) {
      formData.append('file', files[0]);
      }
    };

    console.log(formData.get('file'));
  ```

  - formData.append 방법은 실패하였는데 handleChangeFileName에서 formData.get를 하면 formData에 저장된 값이 출력되지만
    해당 함수 밖에서 console.log를 찍어보면 값이 null이 된다.
    사실 아직 정확한 이유를 찾지는 못했지만 change 이벤트나 click 이벤트 중 어떠한 이유에 의해 리렌더링이 발생하고 formData에 저장된
    값이 초기화 되는 것으로 보인다.

  ### b. useRef

  ```
  const formDataRef = useRef(new FormData());

   const handleChangeFileName = (e: React.ChangeEvent<HTMLInputElement>) => {
    const { files } = e.target;

    if (files && files.length) {
      formDataRef.current.append('file', files[0]);
    }
  };

  console.log('get', formDataRef.current.get('file'));

  return(
    <input onChange={handleChangeFileName} ref={formDataRef}></input>
  )
  ```

  - useRef를 이용한 방법은 성공적으로 값을 저장하고 전달하게 된다.
    다만, Ref를 사용해서 dom에 접근한다는 부분이 좀 지저분하게 보일 수 있다.

  ### c. useMemo

  ```
  const formData = useMemo(() => new FormData(), []);

      const handleChangeFileName = (e: React.ChangeEvent<HTMLInputElement>) => {
        const { files } = e.target;

        if (files && files.length) {
          formData.append('file', files[0]);
        }
      };

    console.log('get', formData.get('file'));
  ```

  - 기존 useRef를 사용한 방법보다 좀 더 깔끔한 방법을 생각해 보다가,
    결국 어떠한 동작에 의해 리렌더링이 발생해도 useRef.current에는 저장된 값이 초기화되지 않아
    기능이 정상적으로 동작한다면 useMemo를 이용하여도 초기화가 이루어지지 않는다고 생각하여 useMemo를 사용하게 되었다.
