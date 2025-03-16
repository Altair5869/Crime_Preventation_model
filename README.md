# Crime_Preventation_model
SVM, Decision Tree, Random Forest을 이용한 범죄 예방 모델

# 🤖 모델 개발 이유
성폭행, 절도, 폭행 등의 범죄가 빈번하게 일어나는 요즈음, 일상 생활에 위협이 되는 범죄로부터 조금이나마 예방할 수 있기를 바람으로 모델을 만들게 되었습니다.

# 📊 데이터 선정
공공 데이터 포털에 있는 경찰청 및 대검찰청 범죄 관련 데이터

공공 데이터 포털: https://www.data.go.kr/

# 📋 가설 수립
- 영가설: 피해자의 나이 및 성별, 범죄가 행해진 장소 및 시간대, 피해 지역, cctv 설치수는 범죄 피해 여부와 관계가 없다.
- 대립가설: 피해자의 나이 및 성별, 범죄가 행해진 장소 및 시간대, 피해 지역, cctv 설치수는 범죄 피해 여부와 관계가 있다.

# 🧑🏻‍💻 데이터 가공
1. 범주형 데이터 변환: 시간, 나이, 성별, 범죄분류, 장소와 관련된 데이터는 범주형의 데이터로 가공

   ex) 절도, 강도, 손괴 etc. : 'T'
   
   살인 미수, 폭행, 상해 etc. : 'K'
   
   강간, 강제추행, 유사 강간 etc. : 'S'
3. 로그 변환: 성별 및 나이, 지역별, 시간대별, 장소별 피해발생건수의 차이가 많이 나 로그변환하여 처리
4. 결측값 및 이상치 처리: 공공 데이터 포털의 데이터라 결측값이 존재하지 않음. 그러나 이상치라고 할 수 있는 '기타' 또는 '알 수 없음' 등의 데이터가 있었고, 이러한 데이터의 양은 타 데이터에 비해 미미하였기에 제거하여 진행
5. 데이터 통합: 성별 및 나이별 범죄 피해 / 장소별 범죄 피해 / 시간대별 범죄 피해 / 지역별 범죄 피해 / cctv 설치수 데이터셋을 하나의 데이터프레임으로 통합

# 데이터 가공 결과
<img width="1150" alt="스크린샷 2025-03-16 오후 5 46 19" src="https://github.com/user-attachments/assets/4bd578e8-25d5-4763-892b-9eb4222e3995" />

# 🦾 모델링
1. Decision Tree:
   - 데이터를 랜덤으로 추출하여 train, test 데이터 생성
   - rpart()를 사용하여 결정트리 모델 생성
   - xerror가 최소인 cp를 선택하여 min_xerror_cp를 만든 후, 가지치기(prune) 수행
   - plot(), text()를 통해 시각화
   - <img width="618" alt="스크린샷 2025-03-16 오후 5 52 43" src="https://github.com/user-attachments/assets/8055a040-a29d-4d58-85f8-9f8c3eaa0110" />
   - 그러나, prune을 수행했음에도 과적합 발생
   - <img width="1056" alt="스크린샷 2025-03-16 오후 5 51 16" src="https://github.com/user-attachments/assets/93104e5a-91d9-4979-8ecf-e53b8fcbb070" />
2. Random Forest:
   - varImpPlot()으로 설명변수 간 중요도 시각화
   - <img width="861" alt="스크린샷 2025-03-16 오후 5 55 10" src="https://github.com/user-attachments/assets/8f90adc4-07d4-4593-bb51-eabb1a9cbe1e" />
   - 최초 모델 호출 시, 과적합 발생. 이후 하이퍼 파라미터의 ntree 개수, nodesize, maxnode 값 수정하여 재호출
   - <img width="949" alt="스크린샷 2025-03-16 오후 5 57 55" src="https://github.com/user-attachments/assets/15f981e3-2678-4760-9f4f-475284becf73" />
   - <img width="1151" alt="스크린샷 2025-03-16 오후 5 57 14" src="https://github.com/user-attachments/assets/0cede168-8e9e-4941-a928-bd3059cd6594" />
3. SVM:
   - 최초 수행 및 커널, 하이퍼 파라미터를 설정했으나 과적합 발생
   - <img width="779" alt="스크린샷 2025-03-16 오후 6 01 00" src="https://github.com/user-attachments/assets/b77bebfb-9fad-4cd3-8d24-14a3eaf8878e" />
   - 이후 하이퍼 파라미터에서 gamma 값 0.01 -> 1로 수정, 그러나 과적합 발생
   - <img width="779" alt="스크린샷 2025-03-16 오후 6 01 26" src="https://github.com/user-attachments/assets/b8f6b51e-c736-43ca-a596-3c836fd79546" />
   - 커널을 polynomial로 설정 후 새로운 SVM 모델 생성
   - <img width="868" alt="스크린샷 2025-03-16 오후 6 02 11" src="https://github.com/user-attachments/assets/3f060fe9-b963-4a3d-b284-f0356cc37de6" />

# 결과
1. 시간대 변수의 영향이 가장 높은 수치를 나타냄
2. 성별, 시간대, 나이, 지역 변수의 높은 영향력
3. 하이퍼 파라미터의 수정에 의한 모델의 과적합 방지
4. 추가적으로 넣은 CCTV 데이터는 미미한 영향
<img width="685" alt="스크린샷 2025-03-16 오후 6 06 48" src="https://github.com/user-attachments/assets/e117c0d2-b1e0-44aa-999e-1bad3028513f" />


# 결론
- 대립가설인 성별, 시간대, 나이, 지역 등의 설명변수가 범죄 피해 여부와 관계가 있음을 증명
- 저녁 시간대의 위험성을 고려하여 여성들의 안심 귀갓길 홀로그램과 같은 안전장치를 늘릴 필요가 있음

