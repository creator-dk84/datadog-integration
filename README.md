# 2024.10.31 Hands-On 환경 구성


본 실습은 Datadog의 App Builder 기능을 다뤄봅니다.

원활한 테스트 환경을 위해, 아래와 같은 사전 구성 단계가 필요합니다.


> 1. AWS 프리티어 계정 생성
> 2. Datadog Trial 계정 생성
> 3. Datadog에 AWS 계정 연동
> 4. Learning Center 계정 생성
> 6. App Builder를 위한 Connection 생성 (Optional)
> 7. App Builder를 위한 IAM 생성 (Optional)
> 8. 인스턴스 생성 (Optional)



### 1. AWS 프리티어 계정 생성
[-> Create AWS Account](https://aws.amazon.com/ko/free/?all-free-tier.sort-by=item.additionalFields.SortRank&all-free-tier.sort-order=asc&awsf.Free%20Tier%20Types=*all&awsf.Free%20Tier%20Categories=*all)
<br>
기존 계정이 있으면 사용 가능합니다.
<br>
<br>
<br>

### 2.Datadog Trial 계정 생성
[-> Create Datadog Trial Account](https://us5.datadoghq.com/signup)
* 계정은 개인 메일 주소를 이용하여 생성하되, 기존 연동 이력이 없는 신규 메일 주소로 생성합니다.
* ⚠️ Region은 United States (US1-East)로 선택합니다.
![image](https://github.com/user-attachments/assets/1b8c3a9b-67c2-4879-93d4-fc63bf361971)

<br>
<br>
<br>

### 3. Datadog에 AWS 계정 연동
- Datadog Console 화면 좌측 하단의 Integrations->Integration로 이동 후, AWS 타일 선택합니다.
![image](https://github.com/user-attachments/assets/c96635b7-6a01-4c66-893a-70d29f7c163a)

![image](https://github.com/user-attachments/assets/ca24c95e-7559-426e-be0f-556f0f659bba)

- \+ Add AWS Account(s) 클릭
- Select AWS Region : ap-northeast-2
- Add Datadog API Key : 자동으로 생성 된 키를 선택하거나, \+ Create New를 이용해 새로운 API 키 생성
- Send AWS Logs to Datadog : Yes
- Enable Cloud Security Management : No
- Launch CloudFormation Template 클릭


이후 AWS의 CloudFormation 화면으로 전환
- 스택 이름과 IAMRoleName은 적절하게 수정
- 다른 부분은 기본값 그대로 적용
- 페이지 하단의 체크박스 버튼에 체크
- 스택 생성 클릭

![image](https://github.com/user-attachments/assets/d79247e9-b1a5-4b08-97d4-c3d282dd4b03)


스택 생성이 완료되면, Datadog 콘솔 화면에서 "Ready!" 버튼 클릭 또는 화면 '새로고침'을 하게되면 연동 된 계정 정보가 표시됩니다.
![image](https://github.com/user-attachments/assets/e713cc40-9206-4b6f-a64c-5d56d4a02397)


화면의 General 탭에서 Include all regions 비활성화 후, ap-northeast-2만 선택합니다.
![image](https://github.com/user-attachments/assets/f59e76d2-6e62-45c8-bba4-80f701b4d260)


Metric Collection 탭에서 Disable All 클릭 후, EC2만 활성화합니다.
![image](https://github.com/user-attachments/assets/c1f82545-a029-4f77-b32b-ef772dacb311)

Limit Metric Collection to Specific Resources 항목에서 EC2 항목에 아래와 같이 태그 삽입합니다.
EC2 | owner:datadog
![image](https://github.com/user-attachments/assets/2c6c97b4-5aa1-4f74-aec6-93be9d129834)

<br>
<br>
<br>


### 4. Learning Center 계정 생성
[-> Create Learning Center Account](https://learn.datadoghq.com/users/sign_up)<br>
Datadog Trial 계정과 관계없이, 사용하고자 하는 메일주소로 회원가입을 진행해주시면 됩니다.


---
<br>
<br>
<br>
이후 항목들은 Optional 부분이며, 실습시간에 다시 진행합니다.

### 5. App Builder를 위한 Connection 생성
- Datadog Console의 Service Mgmt -> App Builder 클릭합니다.
![image](https://github.com/user-attachments/assets/f47954ab-22c9-4179-8a05-ae986ce35775)

- Connections 탭에서 우측의 \+ New Connection 클릭합니다.
- Connections는 Datadog이 다양한 외부 솔루션을 컨트롤하기 위해 인증을 설정하는 부분입니다.
- AWS 선택
![image](https://github.com/user-attachments/assets/f83161db-b79e-4953-9580-55ae5229bd7b)
![image](https://github.com/user-attachments/assets/08c7427c-57a2-430c-a3e1-189948320390)


![image](https://github.com/user-attachments/assets/5f590263-5086-4a95-9d8e-475be20faa75)


### 6. App Builder를 위한 IAM 생성
- 4번의 Connections 단계에서 생성된 코드를 사용자 지정 신뢰정책에 입력합니다.
![image](https://github.com/user-attachments/assets/4d8a749a-3b2c-4ace-b034-db9b27d56834)
- 권한 정책 부분 SKIP
- 역할 이름에 4번 단계에서 지정한 "AWS Role Name" 입력
- 역할 생성
- 생성 된 역할 클릭 후, 권한 추가 (인라인 정책 생성)
![image](https://github.com/user-attachments/assets/65ea424c-39ac-4d0e-94b2-1412f2713388)
- [AWS Permission](https://docs.datadoghq.com/ko/integrations/guide/aws-manual-setup/?tab=roledelegation#aws-integration-iam-policy) 입력
![image](https://github.com/user-attachments/assets/092437e5-0604-4ec0-8128-6f95e5d27bc6)


### 7. 인스턴스 생성
- ⚠️ 인스턴스의 tag 정보에 owner:datadog 입력

---
<br>
<br>


### 8. 정리

세션 이후, 실습을 위해 Datadog에 연동 된 AWS 계정은 비용이 발생할 수 있으니 반드시 삭제하여 주시기 바랍니다.<br>
Datadog Console 좌측 하단의 Integrations -> Integrations로 이동 후, AWS Tile을 클릭합니다.
![image](https://github.com/user-attachments/assets/bade2327-b89f-4b11-ae48-90f40d0709f9)

이후 페이지 하단의 'Delete Account'를 클릭하여, Datadog에 연결 된 AWS 계정을 연동해제 합니다.

![image](https://github.com/user-attachments/assets/a4f50917-4f17-4b27-86a7-0cc2cd0ad619)
