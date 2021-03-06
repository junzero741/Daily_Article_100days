8월 11일 수요일

링크 : https://blog.banksalad.com/tech/build-a-website-with-gatsby/



# 채용 사이트를 만드는게 목표였던 뱅샐
* 뱅샐은 매 프로젝트 때마다 프로덕트 스펙 문서를 정리해본다.
* 검색엔진 최적화, 비개발자의 웹페이지 관리 용이성이 가장 중요했다고.

# GatsbyJS 를 사용한 뱅크샐러드
* Gatsby는 SEO와 성능 두 마리 토끼를 쉽게 잡을 수 있다.
* Gatsby 는 React와 달리 개발 후 Build 과정에서 마크업이 생성된다.
* 즉 페이지 내 모든 콘텐츠가 이미 생성되어 있기에 SEO를 잘 챙겨갈 수 있는 것.
* 필요에따라 CSR, SSR, lazy loading을 적절히 섞을 수도 있어 성능도 단순 static 페이지보다 좋다.
* 프로젝트 폴더 내에서 Markdown 이나 텍스트 에디터가 포함된 CMS로 쉽게 콘텐츠를 관리할 수 있다.


# 문서 공유를 통한 new Stack 도입
* 개발 이유, 목표, 계획, 우려되는 부분, 질문 등을 포함한 테크 스팩 문서를 전사적으로 공유


# 관심사의 분리
* 리액트의 개발방법(컴포넌트 주도 개발)과 매우 유사하므로, 다른 사람이 와서 코드를 만지기도 용이하다.


# 다양한 플러그인 지원
* sitemap을 만들기 위한 gatsby-plugin-sitemap 등 개발 편의를 위한 플러그인들이 매우 다양하다.


# Metadata로 DB 대체
* gatsby-transformer-remark 라는 플러그인으로 markdown 문서를 html 또는 원문, metadata로 가져올 수 있다.


# GatsbyJS 를 쓰며 유의할 점
* 개발 중 동작 과정과 빌드 후 동작 과정이 달라서 생기는 이슈들이 있다.
* 빌드할 땐 window 객체를 사용할 수 없다는 점(빌드 중에는 브라우저 환경이 아닌 Node 환경에서 빌드를 하기 때문) ->  우회 방법은 본문 참고.
* 사이트 최초 진입 페이지 Render와 그 이후 Render가 다르다는 점(빌드 이후에 만들어진 페이지에서 JS로 DOM 조작을 하는 경우) -> 미디어 쿼리 사용 강추(본문 참고) 
