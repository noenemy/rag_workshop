## Introduction to ESEP Gen AI RAG Workshop

이 실습에서는 AWS Gen AI 서비스를 이용해서 RAG를 구현하는 예제를 알아보도록 하겠습니다.

다음 실습 내용을 포함합니다. 

<li>Amazon Q Business를 이용한 RAG 실습</li>
<li>Amazon Bedrock을 이용한 RAG 실습</li>

<img src="https://docs.aws.amazon.com/images/sagemaker/latest/dg/images/jumpstart/jumpstart-fm-rag.jpg" width="600">

# 검색-증강 생성이란 무엇인가요?

RAG(Retrieval-Augmented Generation)는 대규모 언어 모델의 출력을 최적화하여 응답을 생성하기 전에 학습 데이터 소스 외부의 신뢰할 수 있는 지식 베이스를 참조하도록 하는 프로세스입니다. 대규모 언어 모델(LLM)은 방대한 양의 데이터를 기반으로 학습되며 수십억 개의 매개 변수를 사용하여 질문에 대한 답변, 언어 번역, 문장 완성과 같은 작업에 대한 독창적인 결과를 생성합니다. RAG는 이미 강력한 LLM의 기능을 특정 도메인이나 조직의 내부 지식 기반으로 확장하므로 모델을 다시 교육할 필요가 없습니다. 이는 LLM 결과를 개선하여 다양한 상황에서 관련성, 정확성 및 유용성을 유지하기 위한 비용 효율적인 접근 방식입니다.

# 검색-증강 생성이 중요한 이유는 무엇인가요?

LLM은 지능형 챗봇 및 기타 자연어 처리(NLP) 애플리케이션을 지원하는 핵심 인공 지능(AI) 기술입니다. 목표는 신뢰할 수 있는 지식 소스를 상호 참조하여 다양한 상황에서 사용자 질문에 답변할 수 있는 봇을 만드는 것입니다. 안타깝게도 LLM 기술의 특성상 LLM 응답에 대한 예측이 불가능합니다. 또한 LLM 훈련 데이터는 정적이며 보유한 지식에 대한 마감일을 도입합니다.

LLM의 알려진 문제점은 다음과 같습니다.

<li>답변이 없을 때 허위 정보를 제공합니다.
<li>사용자가 구체적이고 최신의 응답을 기대할 때 오래되었거나 일반적인 정보를 제공합니다.
<li>신뢰할 수 없는 출처로부터 응답을 생성합니다.
<li>용어 혼동으로 인해 응답이 정확하지 않습니다. 다양한 훈련 소스가 동일한 용어를 사용하여 서로 다른 내용을 설명합니다.
<li>대형 언어 모델은 현재 상황에 대한 최신 정보를 얻기를 거부하지만 항상 절대적인 자신감을 가지고 모든 질문에 답변하는 지나치게 열정적인 신입 사원으로 생각할 수 있습니다. 안타깝게도 이러한 태도는 사용자 신뢰에 부정적인 영향을 미칠 수 있으며 챗봇이 모방하기를 원하지 않습니다!

RAG는 이러한 문제 중 일부를 해결하기 위한 한 가지 접근 방식입니다. LLM을 리디렉션하여 신뢰할 수 있는 사전 결정된 지식 출처에서 관련 정보를 검색합니다. 조직은 생성된 텍스트 출력을 더 잘 제어할 수 있으며 사용자는 LLM이 응답을 생성하는 방식에 대한 통찰력을 얻을 수 있습니다.

# 검색-증강 생성은 어떻게 작동하나요?
RAG가 없는 경우 LLM은 사용자 입력을 받아 훈련한 정보 또는 이미 알고 있는 정보를 기반으로 응답을 생성합니다. RAG에는 사용자 입력을 활용하여 먼저 새 데이터 소스에서 정보를 가져오는 정보 검색 구성 요소가 도입되었습니다. 사용자 쿼리와 관련 정보가 모두 LLM에 제공됩니다. LLM은 새로운 지식과 학습 데이터를 사용하여 더 나은 응답을 생성합니다. 다음 섹션은 프로세스의 개요를 제공합니다.

**외부 데이터 생성**

LLM의 원래 학습 데이터 세트 외부에 있는 새 데이터를 외부 데이터라고 합니다. API, 데이터베이스 또는 문서 리포지토리와 같은 여러 데이터 소스에서 가져올 수 있습니다. 데이터는 파일, 데이터베이스 레코드 또는 긴 형식의 텍스트와 같은 다양한 형식으로 존재할 수 있습니다. 임베딩 언어 모델이라고 하는 또 다른 AI 기법은 데이터를 수치로 변환하고 벡터 데이터베이스에 저장합니다. 이 프로세스는 생성형 AI 모델이 이해할 수 있는 지식 라이브러리를 생성합니다.

**관련 정보 검색**

관련성 검색을 수행하는 단계는 다음과 같습니다. 사용자 쿼리는 벡터 표현으로 변환되고 벡터 데이터베이스와 매칭됩니다. 예를 들어 조직의 인사 관련 질문에 답변할 수 있는 스마트 챗봇을 생각할 수 있습니다. 직원이 “연차휴가는 얼마나 남았나요?“라고 검색하면 시스템은 개별 직원의 과거 휴가 기록과 함께 연차 휴가 정책 문서를 검색합니다. 이러한 특정 문서는 직원이 입력한 내용과 매우 관련이 있기 때문에 반환됩니다. 관련성은 수학적 벡터 계산 및 표현을 사용하여 계산되고 설정됩니다.

**LLM 프롬프트 확장**

다음으로 RAG 모델은 검색된 관련 데이터를 컨텍스트에 추가하여 사용자 입력(또는 프롬프트)을 보강합니다. 이 단계에서는 신속한 엔지니어링 기술을 사용하여 LLM과 효과적으로 통신합니다. 확장된 프롬프트를 사용하면 대규모 언어 모델이 사용자 쿼리에 대한 정확한 답변을 생성할 수 있습니다.

**외부 데이터 업데이트**

외부 데이터가 오래될 발생하는 상황이 다음 질문이 될 수 있습니다. 검색을 위해 최신 정보를 유지하기 위해 문서를 비동기적으로 업데이트하고 문서의 임베딩 표현을 업데이트합니다. 자동화된 실시간 프로세스 또는 주기적 배치 처리를 통해 이 작업을 수행할 수 있습니다. 이는 데이터 분석에서 흔히 발생하는 과제입니다. 변경 관리에 다양한 데이터 과학 접근 방식을 사용할 수 있기 때문입니다.

