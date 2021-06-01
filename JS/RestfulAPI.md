# ReatfulAPI

## Restful API:
- http통신에서 어떤 자원에 대한 CRUD요청을 Resource와 Method로 표현하여 특정한 형태로 전달하는 방식
- REST란 어떤 자원에 대해 CRUD(Create, Read, Update, Delete) 연산을 수행하기 위해 URI(Resource)로 요청을 보내는 것
- Get, Post 등의 방식(Method)을 사용하여 요청, 요청을 위한 자원은 특정한 형태(Representation of Resource)로 표현
- CRUD 연산에 대한 요청
    - 요청을 위한 Resource(자원, URI)와 이에 대한 Method(행위, POST) 그리고 Representation of Resource(자원의 형태, JSON)을 사용하면 표현이 명확해지므로 이를 REST라 한다


## RESTful API의 구성요소
- **Resource**서버는 Unique한 ID를 가지는 Resource를 가지고 있으며, 클라이언트는 이러한 Resource에 요청을 보냅니다. 이러한 Resource는 URI에 해당합니다.
- **Method**서버에 요청을 보내기 위한 방식으로 GET, POST, PUT, PATCH, DELETE가 있습니다. CRUD 연산 중에서 처리를 위한 연산에 맞는 Method를 사용하여 서버에 요청을 보내야 합니다.(GET과 POST의 차이는 [여기](http://mangkyu.tistory.com/17?category=761303))
- **Representation of Resource**클라이언트와 서버가 데이터를 주고받는 형태로 json, xml, text, rss 등이 있습니다. 최근에는 Key, Value를 활용하는 json을 주로 사용합니다.

## Q) URI 과 URL의 차이점은?
- URL은 Uniform Resource Locator로 인터넷 상 자원의 위치를 의미합니다. 자원의 위치라는 것은 결국 어떤 파일의 위치를 의미합니다. 반면에 URI는 Uniform Resource Identifier로 인터넷 상의 자원을 식별하기 위한 문자열의 구성으로, URI는 URL을 포함하게 됩니다. 그러므로 URI가 보다 포괄적인 범위라고 할 수 있습니다.