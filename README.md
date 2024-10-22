# 2024.10.31 Hands-On 환경 구성


본 실습은 Datadog의 App Builder 기능을 다뤄봅니다.

원활한 테스트 환경을 위해, 아래와 같은 사전 구성 단계가 필요합니다.


> 1. AWS 프리티어 계정 생성
> 2. Datadog Trial 계정 생성
> 3. Datadog에 AWS 계정 연동
> 4. App Builder를 위한 Connection 생성
> 5. App Builder를 위한 IAM 생성
> 6. 인스턴스 생성




1. [AWS 프리티어 계정 생성](https://aws.amazon.com/ko/free/?all-free-tier.sort-by=item.additionalFields.SortRank&all-free-tier.sort-order=asc&awsf.Free%20Tier%20Types=*all&awsf.Free%20Tier%20Categories=*all)
*(기존 계정이 있으면 사용 가능)*


2. [Datadog Trial 계정 생성](https://us5.datadoghq.com/signup)
* 계정은 개인 메일 주소를 이용하여 생성
* ⚠️ Region은 United States (US1-East)로 선택
![image](https://github.com/user-attachments/assets/1b8c3a9b-67c2-4879-93d4-fc63bf361971)


3. Datadog에 AWS 계정 연동
- 좌측 하단의 Integrations->Integration로 이동 후, AWS 타일 선택
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


스택 생성이 완료되면, Datadog 콘솔 화면에서 "Ready!" 버튼 클릭 또는 화면 Refresh

General 탭에서 Include all regions 비활성화 후, ap-northeast-2만 선택
![image](https://github.com/user-attachments/assets/f59e76d2-6e62-45c8-bba4-80f701b4d260)


Metric Collection 탭에서 Disable All 클릭 후, EC2만 활성화
![image](https://github.com/user-attachments/assets/c1f82545-a029-4f77-b32b-ef772dacb311)

Limit Metric Collection to Specific Resources 항목에서 EC2 항목에 아래와 같이 태그 삽입
![image](https://github.com/user-attachments/assets/2c6c97b4-5aa1-4f74-aec6-93be9d129834)



4. App Builder를 위한 Connection 생성
- Service Mgmt -> App Builder 클릭
![image](https://github.com/user-attachments/assets/f47954ab-22c9-4179-8a05-ae986ce35775)

- Connections 탭에서 우측의 \+ New Connection 클릭
- AWS 선택
![image](https://github.com/user-attachments/assets/f83161db-b79e-4953-9580-55ae5229bd7b)
![image](https://github.com/user-attachments/assets/08c7427c-57a2-430c-a3e1-189948320390)


![image](https://github.com/user-attachments/assets/5f590263-5086-4a95-9d8e-475be20faa75)


5. App Builder를 위한 IAM 생성
- 4번의 Connections 단계에서 생성된 코드를 사용자 지정 신뢰정책에 입력
![image](https://github.com/user-attachments/assets/4d8a749a-3b2c-4ace-b034-db9b27d56834)
- 권한 정책 부분 SKIP
- 역할 이름에 4번 단계에서 지정한 "AWS Role Name" 입력
- 역할 생성
- 생성 된 역할 클릭 후, 권한 추가 (인라인 정책 생성)
![image](https://github.com/user-attachments/assets/65ea424c-39ac-4d0e-94b2-1412f2713388)
- [AWS Permission](https://docs.datadoghq.com/ko/integrations/guide/aws-manual-setup/?tab=roledelegation#aws-integration-iam-policy) 입력
![image](https://github.com/user-attachments/assets/092437e5-0604-4ec0-8128-6f95e5d27bc6)


6. 인스턴스 생성


