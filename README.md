# SimpleEQ

## 개발 환경
- OS: macOS
- IDE: Xcode

\* 참고: [freeCodeCamp](https://youtu.be/i_Iq4_Kd7Rc?si=ZgJYunOA4IbsXTyL)

## 실행 방법
1. JUCE framework 다운로드 [공식 문서](https://docs.juce.com/master/tutorial_new_projucer_project.html) [공식 다운로드](https://juce.com/download/)
2. 프로젝트 실행 전 Projucer > Global Paths...에서 다음과 같이 경로 지정
![Global Path](./Images/GlobalPath.png)
3. SimpleEQ.jucer 실행
4. Selected exporter에서 적절한 IDE 선택 후 실행
5. Standalone으로 프로젝트 빌드 시 실행 가능

## 발견한 점

### `juce::ParameterID` 관련
![AudioProcessorParameter](./Images/AudioProcessorParameter.png)
- Logic Pro와 GarageBand는 파라미터 ID 대신 인덱스를 사용하여 파라미터를 관리하기 때문에 파라미터를 추가하거나 삭제하면 자동화 데이터가 손상될 수 있음
- 파라미터 추가 시 version hint를 사용하여 새로운 파라미터들이 항상 이전 파라미터들보다 높은 버전 힌트를 가지도록 설정해야 함
- 파라미터를 삭제하는 것은 위험하며, 특히 Logic Pro와 GarageBand와 같은 DAW에서는 파라미터 삭제로 인해 자동화가 깨질 수 있으므로 가능한 한 피해야 함
- 코드 예시
```Cpp
juce::AudioParameterFloat("Param1", "Param1", 1.f, 1.f);    // Deprecated
juce::AudioParameterFloat(juce::ParameterID("Param1", 1), "Param1", 1.f, 1.f);
```
