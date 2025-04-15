# 실습 1. Amazon Q Business로 RAG 구현하기

프롬프트 엔지니어링(Prompt Engineering)은 대규모 언어 모델(Large Language Model, LLM)과 같은 AI 시스템에게 적절한 프롬프트(prompt)를 제공하여 원하는 결과를 얻도록 하는 기술을 말합니다. 적절한 프롬프트를 만들기 위해서는 다음과 같은 요소를 고려해야 합니다.

1. **문제 정의**: 해결하고자 하는 문제를 명확히 정의하고 모델이 이해할 수 있는 형태로 표현합니다.

2. **데이터 준비**: 모델이 학습한 데이터와 유사한 형태로 입력 데이터를 준비합니다.

3. **프롬프트 디자인**: 모델의 특성과 제약사항을 고려하여 효과적인 프롬프트를 설계합니다. 이를 위해 프롬프트의 길이, 구조, 지시어 등을 최적화합니다.

4. **프롬프트 튜닝**: 모델의 출력을 평가하고 프롬프트를 반복적으로 개선하여 원하는 결과를 얻을 수 있도록 합니다.

5. **결과 해석**: 모델의 출력을 적절하게 해석하고 활용할 수 있도록 합니다.

프롬프트 엔지니어링은 LLM과 같은 강력한 AI 모델의 성능을 최대한 활용하기 위해 필수적인 기술입니다. 효과적인 프롬프트 설계를 통해 모델의 역량을 극대화하고 다양한 분야에 활용할 수 있습니다.

<br>

## 1. Review AWS IAM Identity Center

1. Open the AWS Management Console for IAM Identity Center 
<img src="images/prompt-3-aug7.png" width="600px">

2. From the left navigation bar click on Users to review the list of sample users created for the workshop.

3. Next click on Groups to review the list of sample groups created for the workshop.


## 2. Updating multi-factored authentication (MFA) settings
For the purpose of the workshop, as the users and emails created are only for illustrative purposes, you may disable MFA for AWS Identity Center.

~~~
We strongly recommend that you adhere to the best practice of enabling multi-factored authentication (MFA) for user authentication in your AWS accounts outside of the sample users created for the workshop purposes.
~~~

1. To disable MFA navigate to Settings, select Authentication tab.


2. Under Multi-factor authentication click Configure button. From the configure window > MFA settings section, select Never (disabled), and click Save changes


## 3. Reset and update user password
This workshop will be using the sample user names created in IDC for sign-on to Amazon Q Business application web experience. Here are the steps to reset and update user password.

1. Navigate to Users page and select the user for whom password is required. Example, select John Doe, and click Reset password located at the top right of the user information page.


2. Select Generate a one-time password and share the password with the user, and click Reset password.


2. Copy and store the temporary password generated.


4. To change the temporary password, open the AWS access portal URL in a new Private (Firefox), Incognito (Chrome), or InPrivate (Edge) web browser window. In the sign-in page, enter the user name (john_doe), and click Next.

~~~
Important
Always use new Private (Firefox), Incognito (Chrome), InPrivate (Edge) web browser window for this step. Ensure any existing Private/Incognito/InPrivate web browser windows are closed before opening a new window.
~~~

5. In the password box, enter the temporary password copied and stored in step (3) above, and click Sign in

6. In the next page enter new password. For this demo set it something easy to remember like re:Invent2024, and click Set new password. Close the Private/Incognito/InPrivate web browser, to ensure the user session and cookies are deleted.



7. Repeat steps (1) through (6) for each of the sample users created for this workshop.

~~~
We strongly recommend that you adhere to the best practice of creating strong, hard to guess passwords outside of the sample users created for the workshop purposes.
~~~
