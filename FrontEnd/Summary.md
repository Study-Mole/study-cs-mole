# FrontEnd Summary

- [브라우저 동작 과정](#브라우저-동작-과정)

## 브라우저 동작 과정
사용자가 브라우저에 도메인 주소를 넣으면 DNS를 통하여 도메인 주소에 대응되는 IP 주소를 얻습니다. 이를 기반으로 HTTP Request가 만들어지고, 해당 IP를 가진 서버에 전송되며 서버에서는 TCP/IP 네트워크 스택을 통해 HTTP Response를 만들어 전송합니다. Critical Rendering Path라고 하는 브라우저에 출력되는 단계가 진행되며 브라우저 동작 과정이 마무리됩니다.