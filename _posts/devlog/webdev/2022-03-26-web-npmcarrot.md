---
layout: post
title: npm package 틸트(~)를 대신해서 캐럿(^) 사용
image:
subtitle: 틸트대신 캐럿을 사용해야 하는 내용의 이해
category: devlog
tags: webdev
accent_image: 
  background: url('/assets/img/blog/jeremy-bishop@0,5x.jpg') center/cover
  overlay: false
accent_color: '#ccc'
theme_color: '#ccc'
description: >
  틸트와 캐럿에 대해서 설명하고, 내용을 이해해보자.
invert_sidebar: true
sitemap: false
comments: true
---

# 틸트 대신해서 캐럿을 사용하기
## 틸트(~)
일단 틸트(`~`)에 대해서 알아보자. <br>
[NPM문서](https://docs.npmjs.com/cli/v8/configuring-npm/package-json#dependencies)에 따르면 npm을 사용할 때 `package.json`에서 버전 명시를 다음과 같이 할 수 있다.
 - `1.2.3`
 - `>1.2.3`
 - `>=1.2.3`
 - `<1.2.3`
 - `<=1.2.3`
 - `~1.2.3`

여기서 가장 많이 사용하는 방식이 틸트(`~`) 방식이고, ``npm install MODULE --save``나 ``npm install MODULE --save-dev``를 사용하면 자동으로 ``package.json``에 의존성을 추가 할 수 있는데, 이 때 기본으로 사용하는 방법이 틸트(`~`) 방식이다. <br>
틸트는 간단히 말하자면 현재 지정한 버전의 마지막 자리 내의 범위에서만 자동으로 업데이트 한다. <br>
 - `~0.0.1` : `>=0.0.1 <0.1.0`
 - `~0.1.1` : `>=0.1.1 <0.2.0`
 - `~0.1` : `>=0.1.0 <0.2.0`
 - `~0` : `>=0.0 <0.1`

그래서 다양한 방식으로 버전을 명시했을 때 위와 같은 범위내에서 자동으로 업데이트를 한다.

---

## 캐럿(^)
그러면 캐럿(`^`)은 틸트와 어떻게 다를까?

캐럿을 설명하기 이전에 먼저 [SemanticVersioning](https://semver.org/lang/ko/) (보통 SemVer라고 부른다)에 대해서 알아야 하는데, Node.js도 그렇고 npm 모듈들은 모두 SemVer를 따르고 있다. SemVer는 ``MAJOR.MINOR.PATCH``의 버저닝을 따르는데 각 의미는 다음과 같다.
 - 주 버전(Major Version): 기존 버전과 호환되지 않게 변경한 경우
   - MAJOR version when you make incompatible API changes
 - 부 버전(Minor version): 기존 버전과 호환되면서 기능이 추가된 경우
   - MINOR version when you add functionality in a backwards-compatible manner
 - 수 버전(Patch version): 기존 버전과 호환되면서 버그를 수정한 경우
   - PATCH version when you make backwards-compatible bug fixes

즉, MAJOR 버전은 API의 호환성이 깨질만한 변경사항을 의미하고 MINOR 버전은 하위호환성을 지키면서 기능이 추가된 것을 의미하고 PATCH 버전은 하위호환성을 지키는 범위내에서 버그가 수정된 것을 의미한다.

**캐럿(^)은 Node.js의 모듈이 이 SemVer의 규약을 따르는 것을 신뢰한다는 가정하에 동작한다.** <br>
그래서 MINOR나 PATCH버전은 하위호환성이 보장되어야 하므로 업데이트를 한다.
 - `^1.0.2` : `>=1.0.2 <2.0`
 - `^1.0` : `>=1.0.0 <2.0`
 - `^1` : `>=1.0.0 <2.0`

그래서 캐럿을 사용했을 때에는 위와 같이 동작한다. <br>
틸트와 비교해보면 차이점이 명확한데, `1.x.x` 내에서는 하위호환성이 보장되므로 그 내에서는 모두 업데이트 하겠다는 의미이다.

하지만! 여기에는 **예외사항**이 있다.
 - `^0.1.2` : `>=0.1.2 <0.2.0`
 - `^0.1` : `>=0.1.0 <0.2.0`
 - `^0` : `>=0.0.0 <1.0.0`
 - `^0.0.1` : `==0.0.1`

**버전이 1.0.0 미만인 경우에는 (SemVer에서는 pre-releases라고 부른다) 상황이 다르다.** <br>
소프트웨어 대부분에서 1.0 버전을 내놓기 전에는 API 변경이 수시로 일어난다. 그래서 `0.1`을 사용하다가 `0.2`를 사용하면 API가 모두 달려졌을 수 있다. 그래서 캐럿(`^`)을 사용할 때, 0.x.x에서는 마치 틸트처럼 동작해서 지정한 버전 자릿수 내에서만 업데이트한다. <br>
그리고 `0.0.x`인 경우에는 하위호환성이 보장되지 않을 가능성이 높으므로 위의 마지막 예시처럼 지정한 버전만을 사용한다.

--------

## 캐럿에 대한 오류!
캐럿은 동작방식을 이해하면 수긍 할만하다.(물론 배포용으로는 어렵지만 이건 틸드도 마찬가지) 하지만 약간 복잡한 부분이 있어서 그냥 틸드를 쓰고자 할 수도 있는데 그럼에도 캐럿의 존재 정도는 알고 있어야 한다. 캐럿은 근래에 추가된 기능이므로 과거 버전의 npm은 캐럿을 이해하지 못한다.(정확히 어느 버전부터 추가되었는지 찾지 못헀다) 그래서 캐럿이 없는 구 버전의 npm에서 캐럿을 사용하면 오류가 발생한다.

구문을 이해하지 못하므로 버전을 찾지 못하고 "Error: No compatible version found"이라는 설치 오류가 발생한다. 이는 ``npm install -g`` npm을 통해서 npm을 최신 버전으로 올리면 해결할 수 있다. 캐럿을 모른 상태에서 저 오류를 보면 좀 당황스러울수 있다.

npm **v1.4.3**부터는 ``npm install MODULE --save``나 ``npm install MODULE --save-dev``를 사용했을 때 기본값이 틸드 대신 캐럿이 되었기 때문에 틸드를 사용하고자 한다면 매번 직접 수정해야 한다. 그리고 본인은 틸드방식을 사용하더라도 참조한 모듈이 다른 모듈을 참조할 때 캐럿을 사용할 수도 있으므로 npm을 최신 버전으로 업데이트하지 않으면 결국 오류가 나서 제대로 설치가 안 될 것이다.


Reference : [npm package.json에서 틸드(~) 대신 캐럿(^) 사용하기](https://blog.outsider.ne.kr/1041)