# Blob란?

Blob는 Binary Large Objects의 약자로 "대용량 바이너리 객체"쯤으로 해석할 수 있다. 이름 그대로 이미지, 동영상 등 단순 텍스트 데이터가 아닌 바이너리 데이터를 담을 수 있는 객체이다. HTML5에 들어서면서 자바스크립트에서도 이러한 파일을 다루어야 할 필요성이 생겨 File API 명세에 정의되었다.


## Blob 데이터 생성

Blob 객체는 new 연산자와 Blob 생성자를 사용해 생성한다. 이 때 첫 번째 인수로는 데이터의 배열을 전달하고 두 번째 인수로는 옵션을 전달한다.

    var blobObj = new Blob(array, options);

첫 번째 인수인 배열의 원소는 ArrayBuffer, ArrayBufferView, Blob, DOMString 중 하나여야 하며, 여러 종류를 섞어서 사용할 수도 있다. 

두 번째 인수인 옵션은 객체인데, 콘텐츠의 MIME 타입 문자열인 type 프로퍼티와 줄바꿈 문자를 설정하는 endings 프로퍼티를 가질 수 있다. endings 프로퍼티에 사용할 수 있는 값은 "transparent"와 "native"가 있는데 "transparent"를 사용하면 원래 있던 줄바꿈 문자를 그대로 사용하며 "native"를 사용하면 사용중인 OS에 맞춰 줄바꿈 문자를 바꾼다. 기본값은 "transparent"이다.

다음은 간단한 HTML 문서 파일를 구성하는 예제이다.

    var htmlCode = ['< !doctype html>','Hello, World!'];

    var htmlBlob = new Blob(htmlCode, {type:'text/html'});

이렇게 구성한 Blob 데이터는 화면에 표시하거나 서버로 전송할 수도 있고 사용자가 다운로드 받게 만들 수도 있다. 이 글에서는 URL을 통해 접근하도록 만드는 방법을 잠시 후에 다루겠다.

## 프로퍼티
Blob 객체에는 객체에 포함된 데이터의 크기를 나타내는 size라는 프로퍼티와 객체의 MIME 타입을 알 수 있는 type이라는 프로퍼티가 있다. 위에서 작성한 htmlBlob 객체를 예로 들면 다음과 같은 결과를 볼 수 있다(실행결과 보기).

    console.log(htmlBlob.size); // 54
    console.log(htmlBlob.type); // "text/html"

## 메소드

Blob 객체의 메소드는 객체 데이터를 원하는 크기로 자르는 slice()가 유일하다. 이 메소드의 문법은 다음과 같다.

    var newBlob = blobObj.slice([start [, end [, contentType]]]);

start : 시작 바이트 위치(기본값 0). 음수값을 주면 파일 끝에서부터 시작 지점으로 주어진 값만큼 이동한 위치에서 시작한다. 예를 들어, 100바이트 크기의 파일이라면 -10을 시작 위치로 주었을 때와 90을 시작 위치로 주었을 때 결과가 같다. 파일 크기보다 큰 수를 입력하면 반환되는 새 Blob 객체의 크기는 0이 된다.

end : 끝 바이트 위치. 시작 위치와 마찬가지로 음수값을 줄 수 있다. 기본값은 파일의 크기와 같다.

contentType : 반환되는 새 Blob 객체의 콘텐트 타입을 설정한다.
일부 구형 브라우저(파이어폭스 12 이전, 안드로이드 내장 브라우저 등)에서는 mozSlice 또는 webkitSlice와 같이 제조사 접두어를 사용해야 할 수 있으므로 주의하자.

## Blob URL
앞에서 말했듯 Blob 객체의 용도는 다양하다. 여기서는 그 중 URL을 통해 접근하는 방법을 다루어 보겠다.

일단 Blob 객체를 만들고 나면 이 객체에 가상의 URL을 부여할 수 있다. 이 방식을 사용하면 Blob는 물론 Blob를 상속한 File 객체에도 URL을 부여할 수 있게 되는데, 사용 방법은 다음과 같이 매우 단순하다.

    URL.createObjectURL(blob);

전역 객체인 URL의 createObjectURL 메소드를 호출하면서 첫 번째 인수로 URL을 만들 Blob 또는 File 객체를 전달하면 된다. 이렇게 만들어진 URL은 다음과 같은 형태로 나타난다.

    blob:550e8400-e29b-41d4-a716-446655440000#aboutABBA