---
title: "IAM User 생성"
weight: 21
pre: "<b>2-1 </b>"
tag:
  - IAM_User
---

{{% notice info %}}
실습 하는 동안 사용할 IAM User를 생성합니다.
{{% /notice %}}

1. AWS Management Console에 로그인 한 뒤 IAM 서비스에 접속합니다.
2. 왼쪽 메뉴에서 Users를 선택합니다.
3. Add user 버튼을 클릭하여 사용자 추가 페이지로 들어갑니다.
4. User name에 `<사용자 이름>` 을 입력하고, Access type에 Programmatic access와
AWS Management Console access 둘 모두를 선택합니다. Console password에 `<패스워드>` 를 입력하고,
마지막 Require password reset의 체크는 해제합니다.
5. **\[Next: Permissions\]** 버튼을 클릭하고 Attach existing policies directly를 선택한 뒤 AdministratorAccess 권한을 추가해줍니다.
6. **\[Next: Review\]** 버튼을 클릭하고 정보를 확인한 뒤 Create user 버튼을 클릭하여 사용자 생성을 완료합니다.
7. Download.csv 버튼을 클릭하여 생성한 사용자의 정보를 다운 받습니다. EC2 설정에 꼭 필요한 파일이므로 기억하기 쉬운 위치에 저장합니다.

