# spam_jorim_nulta
본 레포지토리는 [nulta](https://github.com/nulta)님의 [fedi_findandkillspam](https://github.com/nulta/fedi_findandkillspam) 도구를 보다 간단히 사용하기 위해 제작되었습니다.

자세한 내용은 해당 레포를 참조해주세요.

## 사용법
1. docker, docker-compose를 설치합니다.
2. 설정을 합니다
    - **config.json.example** 파일을 **config.json**으로 복사합니다.
    - **config.json**을 열고 값을 수정합니다
    ```jsonc
    {
        "Mastodon": false,  // <-- 마스토돈이라면, true로 설정하세요
        "Misskey": false,   // <-- 미스키라면, true로 설정하세요

        // 미스키에서, 스팸을 올린 사용자를 자동으로 정지할지 정합니다.
        // false일 경우 노트만 삭제하고 사용자를 정지하지는 않습니다.
        // (마스토돈에서는 노트 삭제를 지원하지 않습니다. 그래서 항상 정지합니다.)
        "Misskey_ShouldBanUser": true,

        "Site": "https://instancename.example.com/",  // <-- 인스턴스의 주소를 넣으세요
        "ApiKey": "???????"  // <-- API 액세스 토큰을 넣으세요 (아래 설명 참조)
    }
    ```
3. `docker-compose up -d`를 사용하여 실행합니다.

### (Misskey) API 액세스 토큰 얻기
- 관리자 권한을 사용하려면, 미스키 API를 통해서 토큰을 발급받아야 합니다.
1. AiScript 스크래치 패드를 엽니다. (/scratchpad)
2. 아래의 AiScript를 실행합니다. 그러면 토큰이 나옵니다.
```
<: "Token is:"
<: Mk:api("miauth/gen-token" {
  session: null,
  name: 'Fedi_FindAndKillSpam',
  description: 'github.com/nulta/fedi_findandkillspam',
  permission: ['read:account', 'write:notes', 'write:admin:suspend-user'],
}).token
```
3. **제어판의 역할 설정에서, 모더레이터의 `요청 빈도 제한`을 `0%`로 설정해 주세요.**
    - 기본값(100%)에서는, 노트를 1시간에 300개까지만 삭제할 수 있습니다. (유저 정지에는 제한이 없습니다.)

### (Mastodon) API 액세스 토큰 얻기
1. 설정 -> 개발 -> 새로운 애플리케이션
2. *애플리케이션 이름*은 마음대로 설정합니다
3. `read:statuses`와 `admin:write:accounts` 권한을 허용하고 제출합니다.
4. *액세스 토큰*을 얻었습니다.

