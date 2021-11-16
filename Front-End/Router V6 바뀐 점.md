#  Router V6 바뀐 점

## 1. 70%나 작아진 번들 사이즈

<img src="./../Image/Router%20v6_번들%20사이즈.png" width="600px" height="400px" alt="Router v6_번들사이즈"></img>

- 번들 사이즈를 줄여야 하는 이유
  - 성능 및 로드 시간의 차이는 End User에게 많은 영향을 미친다

- 예시
  - Akami의 연구에 따르면 사이트를 로드하는 데 4초 이상 걸리면 사용자의 25% 이상이 사이트를 이탈하게 됩니다
  - 2010년에 Strangeloop Networks는 Pinterest는 인지 대기 시간을 40% 줄였을 때 검색 엔진 트래픽과 가입을 15% 증가 시켰습니다
  - BBC는 사이트를로드하는 데 1 초가 더 소요될 때마다 10%의 추가 사용자를 잃었다는 사실을 발견했습니다

## 2. Switch -> Routes 이름 변경

```
// v5
<Switch>
  <Route path='/' />
</Switch>

// v6
<Routes>
  <Route path='/' />
</Routes>
```

## 3. exact는 더 이상 사용하지 않는다

## 4. component -> element 이름 변경

```
// v5
<Switch>
  <Route exact path='/' component={Main} />
  <Route path='/login' component={Login} />
  <Route path='/post' component={Post} />
</Switch>

// v6
<Routes>
  <Route exact path='/' element={<Main />} />
  <Route path='/login' element={<Login />} />
  <Route path='/post' element={<Post />} />
</Routes>
```
