**Volume 06. Physical AI Hardware Software Co Design**

# Chapter 05. Edge Cloud Split Architecture

## 05.01. Why Physical AI Needs Hierarchical Compute

![](images/image1.png){width="7.268055555555556in" height="7.268055555555556in"}

물리 인공지능(Physical AI)은 물리적 상호작용(physical interaction)의 속도와 현대 지능 시스템(modern intelligence system)의 계산 복잡성(computational complexity) 사이에 존재하는 근본적인 불일치 조건에서 동작한다. 모터(motor), 안전 제어기(safety controller), 안정화 루프(stabilization loop)는 수 밀리초(milliseconds) 이내의 응답을 요구할 수 있지만, 인식(perception), 월드 모델링(world modeling), 계획(planning), 추론(reasoning)은 훨씬 더 큰 계산 부하(computational workload)를 필요로 한다. 하나의 컴퓨팅 계층(computing layer)만으로 이러한 모든 요구사항을 효율적으로 충족하기 어렵기 때문에 계층형 컴퓨팅(hierarchical compute)은 자연스러운 시스템 아키텍처(system architecture)가 된다.

가장 낮은 계층에서 물리 시스템(physical system)은 센서(sensor)와 액추에이터(actuator)에 가까운 위치에서 결정론적(deterministic)이고 높은 신뢰성을 갖는 컴퓨팅(computing)을 필요로 한다. 모터 전류 제어(motor current control), 관절 안정화(joint stabilization), 비상 정지(emergency stopping), 감시 기능(watchdog function), 기본 안전 로직(safety logic)은 원격 서버(remote server)나 예측하기 어려운 네트워크 통신(network communication)에 의존할 수 없다. 따라서 마이크로컨트롤러(microcontroller), 실시간 프로세서(real-time processor), 전용 제어 하드웨어(dedicated control hardware)는 상위 수준의 인공지능 자원(AI resource)을 사용할 수 없는 상황에서도 계속 동작하는 로컬 계산 기반(local computational foundation)을 형성한다.

이러한 제어 계층(control layer) 위에서 로봇 탑재 엣지 컴퓨팅(on-robot edge computing)은 환경과 즉각적으로 상호작용하는 데 필요한 지능(intelligence)을 제공한다. 카메라(camera), 라이다(LiDAR), 레이더(radar), 관성측정장치(IMU), 고유수용성 센서(proprioceptive sensor)는 지속적으로 데이터를 생성하며, 이 데이터는 객체(object), 자유 공간(free space), 움직임(motion), 위치 추정(localization), 로봇 상태(robot state)에 대한 추정값으로 변환되어야 한다. GPU, NPU 또는 이기종 엣지 플랫폼(heterogeneous edge platform)은 이러한 인식 및 추론 작업(perception and inference workload)을 로봇 가까이에서 처리하면서 실제적인 지연시간(latency) 요구사항을 충족할 수 있게 한다.

엣지 계층(edge layer)은 외부 인프라(external infrastructure)와 통신하기에는 결과가 너무 빠르게 전개되는 의사결정(decision)도 지원한다. 장애물 회피(obstacle avoidance), 로컬 궤적 생성(local trajectory generation), 조작 보정(manipulation correction), 지형 평가(terrain assessment), 단기 예측(short-horizon prediction)은 무선 연결(wireless connectivity)이 느려지거나 완전히 사라지는 상황에서도 계속 수행되어야 한다. 따라서 계층형 컴퓨팅(hierarchical compute)은 자율 엣지 지능(autonomous edge intelligence)을 단순히 클라우드 트래픽(cloud traffic)을 줄이기 위한 최적화가 아니라 실제 운용을 위한 필수 요구사항(operational requirement)으로 간주한다.

그러나 모든 인공지능 작업(AI workload)을 로봇 내부에 배치하는 것은 비효율적이며 물리적으로도 현실적이지 않은 경우가 많다. 대규모 월드 모델(large world model), 파운데이션 모델(foundation model), 플릿 규모 최적화(fleet-scale optimization), 장기 추론(long-horizon reasoning), 시뮬레이션(simulation), 모델 학습(model training), 대규모 과거 데이터 분석(historical-data analysis)은 모바일 플랫폼(mobile platform)이 제공할 수 있는 수준보다 훨씬 많은 메모리(memory), 가속기 용량(accelerator capacity), 저장장치(storage), 냉각 능력(cooling capacity)을 요구할 수 있다. 특히 전력 소비(power consumption)와 열 방출(thermal dissipation)은 고성능 컴퓨팅(high-performance computing)을 로봇에 지속적으로 탑재하는 데 강력한 제약조건으로 작용한다.

온프레미스 컴퓨팅 계층(on-premise computing layer)은 개별 로봇(individual robot)과 원격 클라우드 인프라(remote cloud infrastructure) 사이의 간극을 연결할 수 있다. 공장(factory), 물류창고(warehouse), 병원(hospital), 캠퍼스(campus), 보안 시설(secured facility) 내부의 로컬 서버(local server)는 여러 로봇으로부터 정보를 통합하고, 공유 지도(shared map)와 운영 지식(operational knowledge)을 유지하며, 더 무거운 인공지능 모델(AI model)을 실행하고, 플릿 행동(fleet behavior)을 조정할 수 있다. 인프라가 지리적으로 가까운 위치에 있기 때문에 원격 클라우드 서비스(cloud service)에서 발생할 수 있는 전체 통신 불확실성(communication uncertainty)을 피하면서 더 높은 계산 능력을 제공할 수 있다.

클라우드 컴퓨팅(cloud computing)은 엄격한 실시간 실행(real-time execution)보다 매우 크고 탄력적인 컴퓨팅 자원(elastic computing resource)의 이점을 활용할 수 있는 작업을 처리하도록 이러한 계층 구조를 확장한다. 대규모 학습(large-scale training), 데이터셋 처리(dataset processing), 시뮬레이션 캠페인(simulation campaign), 모델 평가(model evaluation), 플릿 전체 분석(fleet-wide analytics), 소프트웨어 배포(software distribution), 장기 지식 통합(long-term knowledge aggregation)을 중앙에서 수행할 수 있다. 그 결과 생성된 모델(model), 정책(policy), 지도(map), 업데이트(update)는 이후 온프레미스 시스템(on-premise system)과 개별 로봇으로 전달되어 배포될 수 있다.

이러한 분리는 서로 다른 계산 시간 척도(computational time scale)를 만들어낸다. 저수준 제어(low-level control)는 초당 수백 또는 수천 회 실행될 수 있고, 인식(perception)과 로컬 계획(local planning)은 초당 수십 회 동작할 수 있으며, 복잡한 추론(sophisticated reasoning)은 수 초에 걸쳐 수행될 수 있다. 플릿 학습(fleet learning)이나 모델 학습(model training)은 수 시간 또는 수 일의 시간 범위에서 진행될 수도 있다. 계층형 컴퓨팅은 각 작업을 해당 작업의 지연시간(latency), 신뢰성(reliability), 메모리(memory), 에너지(energy), 계산 요구사항(computational requirement)을 가장 효율적으로 충족할 수 있는 위치에서 실행하도록 한다.

데이터 이동(data movement)은 계층 구조가 필요한 또 다른 이유이다. 현대의 물리 인공지능 플랫폼(Physical AI platform)은 이미지(image), 포인트 클라우드(point cloud), 레이더 측정값(radar measurement), 텔레메트리(telemetry), 내부 모델 상태(internal model state)로 구성된 방대한 데이터 스트림(data stream)을 생성할 수 있다. 모든 원시 센서 샘플(raw sensor sample)을 외부 인프라로 지속적으로 전송하면 상당한 통신 대역폭(bandwidth)과 에너지를 소비한다. 대신 엣지 시스템(edge system)은 관측 데이터(observation)를 로컬에서 필터링(filtering), 압축(compression), 요약(summarization), 해석(interpretation)하고 플릿 학습(fleet learning), 진단(diagnostics), 모델 개선(model improvement)에 장기적인 가치가 있는 정보만 전송할 수 있다.

계층형 컴퓨팅(hierarchical computation)은 시스템 복원력(resilience)도 향상시킨다. 로봇은 온프레미스 서버(on-premise server) 또는 클라우드 서비스(cloud service)와의 통신이 중단되었다는 이유만으로 안전하게 동작할 수 없는 상태가 되어서는 안 된다. 필수적인 인식(perception), 위치 추정(localization), 계획(planning), 제어(control) 기능은 로컬에서 계속 사용할 수 있어야 하며, 외부 컴퓨팅 자원(external computing resource)은 최소 자율 능력(minimum autonomous capability)을 정의하기보다는 이를 강화하는 역할을 해야 한다. 연결이 복구되면 동기화(synchronization)를 재개하고 축적된 데이터를 보다 광범위한 학습 프로세스(learning process)에 통합할 수 있다.

따라서 이 아키텍처(architecture)는 명확한 의존성 계층(dependency hierarchy)을 형성한다. 저수준 제어(low-level control)는 로봇 외부의 요소에 거의 의존하지 않아야 하고, 엣지 자율성(edge autonomy)은 주로 온보드 자원(onboard resource)에 의존해야 한다. 온프레미스 지능(on-premise intelligence)은 여러 로봇을 조정하고 기능을 강화할 수 있으며, 클라우드 인프라(cloud infrastructure)는 대규모 학습 및 생애주기 서비스(lifecycle service)를 제공할 수 있다. 상위 계층은 지능과 효율성을 향상시키지만, 이러한 서비스를 사용할 수 없는 경우에도 하위 계층은 로봇의 운용성과 안전성을 유지한다.

안전성(safety)은 이러한 계층 구성을 더욱 강화한다. 서로 다른 계산 기능(computational function)은 서로 다른 수준의 중요도(criticality)를 갖기 때문이다. 비상 제동(emergency braking)이나 액추에이터 보호(actuator protection)는 선택적인 의미론적 추론(semantic reasoning)이나 플릿 분석(fleet analytics)과 동일한 고장 가정(failure assumption)을 적용할 수 없다. 안전 중요 제어(safety-critical control)를 계산 집약적인 인공지능(computationally intensive AI)과 분리하면 고장을 격리하고 독립적인 폴백 메커니즘(fallback mechanism)을 구현할 수 있다. 따라서 상위 수준 모델(high-level model)의 성능이 저하되더라도 기본적인 안정화(stabilization)나 보호 동작(protective behavior)이 반드시 손상되는 것은 아니다.

계층형 컴퓨팅은 계산 자원(computational resource)이 과부하될 때 점진적 성능 저하(graceful degradation)를 구현할 수도 있다. 로봇은 필수적인 제어(control)와 충돌 회피(collision avoidance)를 유지하면서 일시적으로 인식 해상도(perception resolution)를 낮추거나, 예측 구간(prediction horizon)을 단축하거나, 계산 비용이 높은 추론 모듈(reasoning module)을 비활성화하거나, 더 단순한 로컬 정책(local policy)을 사용할 수 있다. 이후 더 충분한 인프라 자원(infrastructure resource)을 사용할 수 있게 되면 상위 수준 기능을 복원할 수 있다. 이를 통해 컴퓨팅 가용성(compute availability)은 전부 아니면 전무인 조건이 아니라 관리 가능한 운용 능력의 연속적 범위(managed spectrum of operational capability)가 된다.

동일한 계층 구조는 확장 가능한 배포(scalable deployment)도 지원한다. 하나의 로봇은 대부분의 기능을 온보드 컴퓨팅(onboard computing)을 통해 수행할 수 있지만, 수십 또는 수백 대의 로봇은 온프레미스 인프라(on-premise infrastructure)를 통해 지도(map), 학습된 표현(learned representation), 교통 정보(traffic information), 작업 할당(task assignment), 운용 경험(operational experience)을 공유할 수 있다. 클라우드 시스템(cloud system)은 다시 여러 사이트(site)의 지식을 통합할 수 있다. 결과적으로 지능은 액추에이터 수준의 즉각적인 반응성(actuator-level responsiveness)에서 로봇 수준 자율성(robot-level autonomy), 플릿 수준 협업(fleet-level coordination), 조직 수준 학습(organization-level learning)으로 확장된다.

모델 배치(model placement)는 인공지능을 단순히 로컬 또는 원격에서 실행할지를 결정하는 문제가 아니라 공동 설계 문제(co-design problem)가 된다. 개발자는 추론 지연시간(inference latency), 통신 지연(communication delay), 모델 크기(model size), 메모리 대역폭(memory bandwidth), 가속기 활용률(accelerator utilization), 전력 소비(power consumption), 안전 중요도(safety criticality), 개인정보 보호 요구사항(privacy requirement), 예상 연결성(expected connectivity)을 함께 고려해야 한다. 하나의 모델도 여러 계층에 분할될 수 있으며, 엣지에서는 압축된 표현(compact representation)을 지속적으로 실행하고 더 큰 모델은 다른 계층에서 필요할 때 더 깊은 추론(deeper reasoning)을 제공할 수 있다.

개인정보 보호(privacy)와 데이터 주권(data sovereignty)은 계산 성능(computational performance)만큼이나 이러한 분할 구조에 영향을 미칠 수 있다. 민감한 카메라 스트림(camera stream), 인간 상호작용 데이터(human interaction data), 산업 정보(industrial information), 독점적인 운용 기록(proprietary operational record)은 로봇 또는 시설 내부에 유지해야 할 수 있다. 온프레미스 인프라(on-premise infrastructure)는 중요한 정보를 로컬에서 통제하면서 중앙집중식 컴퓨팅(centralized computing)의 많은 장점을 활용할 수 있게 하며, 선택된 표현(selected representation)이나 승인된 데이터셋(approved dataset)만 외부 클라우드 환경으로 전달하도록 구성할 수 있다.

따라서 계층형 컴퓨팅(hierarchical compute)은 지능의 물리적 특성(physical nature of intelligence)에 대응하기 위한 아키텍처적 해법(architectural response)으로 이해해야 한다. 물리 인공지능(Physical AI)은 밀리초 단위로 반응하면서 동시에 더 긴 시간 범위에 걸쳐 추론하고, 축적된 경험으로부터 학습하며, 여러 기계를 조정하고, 통신 장애를 견디며, 엄격한 전력 및 열 제약(power and thermal constraint) 안에서 동작해야 한다. 이러한 요구사항은 지능을 하나의 컴퓨팅 위치에 집중시키는 대신 제어(control), 엣지(edge), 온프레미스(on-premise), 클라우드(cloud) 자원으로 자연스럽게 분산시킨다.

결과적으로 이러한 시스템은 계층화된 계산 신경계(layered computational nervous system)와 유사한 구조를 갖는다. 빠른 로컬 루프(local loop)는 즉각적인 물리적 안정성(physical stability)을 유지하고, 온보드 인공지능(onboard AI)은 주변 세계를 해석하고 대응하며, 가까운 인프라는 보다 깊은 공유 지능(shared intelligence)을 제공한다. 대규모 원격 자원(remote resource)은 더 긴 시간 및 조직적 범위에서의 학습을 지원한다. 정보와 모델은 이러한 계층 사이를 이동하지만, 각 계산의 긴급성과 물리적 결과에 따라 책임이 적절한 계층에 배치된다.

궁극적으로 계층형 컴퓨팅(hierarchical compute)은 물리 인공지능(Physical AI)이 실시간 자율성(real-time autonomy)과 계산 확장성(computational scalability)을 동시에 확보하도록 한다. 엣지 지능(edge intelligence)은 기계의 반응성과 독립성을 유지하고, 온프레미스 자원(on-premise resource)은 추론과 플릿 협업(fleet coordination)을 확장하며, 클라우드 인프라(cloud infrastructure)는 대규모 학습과 지식 축적(knowledge accumulation)을 가능하게 한다. 이러한 엣지--온프레미스--클라우드(edge--on-premise--cloud) 구조는 이후 다루게 될 작업 분할(workload partitioning), 연결성(connectivity), 공유 지능(shared intelligence), 보안(security), 확장 가능한 물리 인공지능 배포(scalable Physical AI deployment)를 위한 기반을 제공한다.

## 05.02. On Robot Edge Intelligence

![](images/image2.png){width="7.268055555555556in" height="7.268055555555556in"}

로봇 탑재 엣지 지능(On-Robot Edge Intelligence)은 즉각적인 자율 행동(autonomous behavior)에 필요한 계산 능력을 물리적 기계 내부에 직접 배치한다. 로봇을 외부 지능(external intelligence)에 연결된 원격 제어 단말로 취급하는 대신, 이러한 아키텍처는 인식(perception), 위치 추정(localization), 세계 이해(world understanding), 계획(planning), 의사결정(decision making), 안전 모니터링(safety monitoring)을 로컬에서 수행할 수 있게 한다. 따라서 저수준 제어(low-level control)와 외부 컴퓨팅 인프라(external computing infrastructure) 사이의 핵심 지능 계층(intelligence layer)을 형성한다.

물리적 상호작용(physical interaction)은 로컬 지능(local intelligence)을 기존의 클라우드 기반 인공지능 추론(cloud-based AI inference)과 근본적으로 다르게 만든다. 로봇은 센서 관측(sensor observation)을 지속적으로 수신하는 동시에 자신의 행동(action)을 통해 환경을 변화시킨다. 각각의 움직임은 새로운 관측을 생성하고 이는 다시 다음 의사결정에 영향을 미친다. 이러한 폐쇄형 인식--행동 루프(closed perception--action loop)는 감지, 추론, 행동 사이의 시간적 관계가 통신 지연(communication delay)에 의해 깨지지 않도록 계산이 물리 시스템 가까이에서 수행될 것을 요구한다.

현대의 로봇은 카메라(camera), 라이다(LiDAR), 레이더(radar), 깊이 센서(depth sensor), 마이크(microphone), 관성측정장치(IMU), 엔코더(encoder), 힘 센서(force sensor), 기타 고유수용성 장치(proprioceptive device)를 통해 상당한 양의 센서 데이터(sensor data)를 생성할 수 있다. 의사결정 전에 이러한 모든 원시 데이터 스트림(raw data stream)을 외부 서버로 전송한다면 대역폭(bandwidth)과 지연시간(latency)에 대한 의존성이 발생한다. 로봇 탑재 엣지 지능은 이러한 정보 대부분을 로컬에서 처리하여 고대역폭 센서 측정값을 주변 환경과 로봇 자체에 대한 압축된 표현(compact representation)으로 변환한다.

따라서 인식(perception)은 엣지(edge)에서 실행되는 가장 중요한 작업 중 하나이다. 비전 네트워크(vision network)는 객체를 탐지하고 추적할 수 있으며, 라이다 처리(LiDAR processing)는 기하학적 구조(geometric structure)를 추정하고, 레이더는 움직임 정보를 제공하며, 멀티모달 융합(multimodal fusion)은 상호 보완적인 관측을 결합할 수 있다. 이러한 계산은 탐지 객체(detected object), 점유 상태(occupancy), 주행 가능성(traversability), 자유 공간(free space), 의미론적 특징(semantic feature), 깊이(depth), 로컬 환경 형상(local environmental geometry)과 같은 표현을 생성하여 상위 자율 기능이 즉시 활용할 수 있도록 한다.

위치 추정(localization)과 상태 추정(state estimation) 역시 로컬 실행(local execution)의 이점을 크게 받는다. 로봇은 자신의 위치, 이동 상태, 내부 구성(configuration)의 변화를 지속적으로 추정해야 한다. 카메라, 라이다, 위성항법시스템(GNSS), 관성측정장치(IMU), 휠 엔코더(wheel encoder), 관절 센서(joint sensor)의 정보를 비교적 높은 갱신 주기(update rate)로 융합해야 할 수 있다. 지연된 상태 추정은 계획과 제어에 오류를 전파할 수 있으므로 일관된 물리적 행동을 유지하기 위해 로컬 처리가 필수적이다.

엣지 지능(edge intelligence)은 점차 기존의 인식 기능만이 아니라 로컬 세계 이해(local world understanding)까지 포함하고 있다. 로봇은 주변 객체, 표면(surface), 행위 주체(agent), 장애물(obstacle), 움직임 패턴(motion pattern), 가능한 미래 상태(future state)에 대한 단기 표현(short-term representation)을 유지할 수 있다. 경량 월드 모델(lightweight world model)은 후보 행동(candidate action) 이후 환경이 어떻게 변화할지를 예측할 수 있다. 이러한 예측 능력은 잠재적으로 비용이 크거나 위험한 행동을 물리 시스템이 실행하기 전에 그 결과를 추론할 수 있도록 한다.

계획(planning)은 이러한 내부 표현(internal representation)을 물리적 행동과 연결한다. 로컬 플래너(local planner)는 장애물, 운동학적 제약(kinematic constraint), 동역학적 한계(dynamic limit), 현재 목표를 고려하면서 궤적(trajectory), 내비게이션 명령(navigation command), 조작 동작(manipulation motion), 행동 시퀀스(action sequence)를 생성할 수 있다. 환경 조건은 빠르게 변화할 수 있기 때문에 계획이 항상 원격 계산을 기다릴 수는 없다. 엣지 실행(edge execution)을 통해 사람이 이동하거나 장애물이 나타나거나 지형이 변하거나 기존 가정이 더 이상 유효하지 않을 때 즉시 궤적을 수정할 수 있다.

의사결정(decision making)과 추론(reasoning)도 사용 가능한 하드웨어에 계산 규모를 맞춘다면 로컬에서 수행할 수 있다. 경량 정책(compact policy), 작업 플래너(task planner), 행동 트리(behavior tree), 학습 기반 의사결정 모델(learned decision model), 최적화된 추론 네트워크(optimized reasoning network)는 현재 관측과 목표로부터 적절한 행동을 선택할 수 있다. 대규모 파운데이션 모델(foundation model)이나 계산 비용이 높은 추론 시스템(reasoning system)은 로봇 외부에 유지하면서, 더 작은 모델이나 압축된 구성 요소가 온보드(onboard)에서 지속적으로 사용할 수 있는 의사결정 능력을 제공할 수 있다.

이러한 구조는 행동 주기(action frequency)와 추론 주기(reasoning frequency) 사이에 중요한 차이를 만든다. 저수준 제어기(low-level controller)는 수백 또는 수천 헤르츠(hertz)로 동작할 수 있지만, 인식과 계획은 더 낮은 주기로 수행되며 복잡한 의미론적 추론(semantic reasoning)은 필요한 경우에만 실행될 수 있다. 로봇 탑재 엣지 지능은 모든 인공지능 기능이 동일한 주기로 실행될 것을 요구하지 않는다. 대신 각 작업이 물리적 행동에 영향을 미치는 속도에 따라 작업 부하(workload)를 스케줄링할 수 있다.

이기종 컴퓨팅(heterogeneous computing)은 이러한 아키텍처에 특히 적합하다. CPU는 운영체제(operating system), 통신(communication), 오케스트레이션(orchestration), 순차 로직(sequential logic)을 관리할 수 있고, GPU는 비전, 멀티모달 네트워크(multimodal network), 월드 모델, 병렬 수치 연산(parallel numerical workload)을 가속할 수 있다. NPU 또는 전용 인공지능 가속기(AI accelerator)는 효율적인 신경망 추론(neural inference)을 제공하며, 마이크로컨트롤러(microcontroller)는 결정론적 제어 기능(deterministic control function)을 유지한다. 따라서 외부 인프라를 고려하기 전부터 로봇 자체가 하나의 분산 컴퓨팅 시스템(distributed computing system)이 된다.

메모리 대역폭(memory bandwidth)과 데이터 이동(data movement)은 종종 단순한 산술 연산 성능(arithmetic performance)만큼 중요하다. 여러 고해상도 센서(high-resolution sensor)는 지속적으로 대규모 데이터 스트림을 생성하며, 이러한 데이터는 인공지능 가속기에 공급되기 전에 전송, 동기화(synchronization), 디코딩(decoding), 변환(transformation) 과정을 거쳐야 한다. 이론적인 계산 성능이 뛰어난 엣지 플랫폼이라도 메모리 대역폭, 인터커넥트(interconnect), 센서 인터페이스(sensor interface), 전처리 파이프라인(preprocessing pipeline)이 필요한 데이터 흐름을 지원하지 못하면 실제 성능은 낮아질 수 있다. 따라서 엣지 지능은 완전한 시스템 관점에서 설계되어야 한다.

전력 소비(power consumption)는 또 하나의 근본적인 제약조건이다. 데이터센터 서버(data-center server)와 달리 모바일 로봇(mobile robot)은 일반적으로 제한된 배터리 용량으로 동작하며, 액추에이터, 센서, 통신 장치, 컴퓨팅 하드웨어가 동일한 에너지 예산(energy budget)을 공유한다. 인공지능 컴퓨팅을 증가시키면 인식이나 추론 능력은 향상될 수 있지만 운용 시간을 단축하고 냉각 요구량을 증가시킬 수 있다. 따라서 엣지 지능의 유용성을 평가하는 기준은 단순한 최대 계산 처리량(compute throughput)이 아니라 단위 에너지당 유용한 자율 능력(useful autonomous capability)이 되어야 한다.

열 관리(thermal management)는 이러한 전력 제약과 밀접하게 연결되어 있다. 지속적인 GPU 또는 가속기 작업은 배터리, 전력 전자장치(power electronics), 기타 온도 민감 부품이 포함된 로봇 내부에서 열을 발생시킨다. 냉각 능력이 충분하지 않으면 계산 요구가 높아지는 바로 그 순간에 열 스로틀링(thermal throttling)이 발생하여 추론 성능이 저하될 수 있다. 따라서 하드웨어 선택, 모델 최적화(model optimization), 작업 스케줄링(workload scheduling), 인클로저 설계(enclosure design), 공기 흐름(airflow), 전도 냉각(conductive cooling)을 함께 고려해야 한다.

안전성(safety)을 위해 엣지 지능은 저수준 제어와 분리되어 있으면서도 긴밀하게 연결되어야 한다. 인공지능은 궤적, 목표 속도(target velocity), 힘(force), 행동을 제안할 수 있지만, 안전 중요 제어기(safety-critical controller)는 명령이 허용된 물리적 한계 안에 있는지를 검증해야 한다. 비상 정지(emergency stopping), 액추에이터 보호(actuator protection), 감시 장치(watchdog), 필수 안정화(essential stabilization)는 인공지능 프로세스가 충돌하거나 과부하되더라도 계속 동작해야 한다. 이러한 분리는 상위 지능이 물리 시스템의 단일 장애점(single point of failure)이 되는 것을 방지한다.

온보드 지능(onboard intelligence)은 성능이 저하된 센싱(degraded sensing)과 불확실한 예측(uncertain prediction)에도 대응해야 한다. 센서는 차폐되거나 오염되고, 포화(saturation)되거나 보정이 틀어지거나 일시적으로 사용할 수 없게 될 수 있다. 신경망 모델(neural model) 역시 신뢰도가 낮거나 잘못된 출력을 생성할 수 있다. 따라서 엣지 소프트웨어(edge software)는 센서 상태(sensor health), 모델 신뢰도(model confidence), 타이밍 동작(timing behavior), 센서 모달리티 사이의 일관성(consistency)을 모니터링해야 한다. 불확실성이 증가하면 속도를 낮추고, 안전 여유(safety margin)를 확대하며, 표현 방식을 변경하거나 중복 센싱(redundant sensing)을 활용하거나 더 단순한 행동으로 폴백(fallback)할 수 있다.

연결성(connectivity)은 엣지 지능을 강화해야 하지만 자율성의 존재 여부를 결정해서는 안 된다. 통신을 사용할 수 있을 때 로봇은 온프레미스(on-premise) 및 클라우드(cloud) 자원으로부터 업데이트된 모델, 공유 지도(shared map), 작업 정보(task information), 플릿 지식(fleet knowledge), 지침(guidance)을 받을 수 있다. 연결이 끊어지더라도 필수 자율 기능은 로컬에서 계속 동작해야 한다. 운용 데이터는 온보드에 버퍼링(buffering)한 뒤 나중에 동기화할 수 있어 간헐적 연결(intermittent connectivity)이나 제한된 네트워크 대역폭에서도 물리 시스템의 유용성을 유지할 수 있다.

이러한 오프라인 기능(offline capability)은 실외 로봇(outdoor robot), 모바일 매니퓰레이터(mobile manipulator), 공중 시스템(aerial system), 산업용 기계(industrial machine), 대규모 시설에서 동작하는 로봇에 특히 중요하다. 무선 통신 범위는 거리, 구조물, 간섭(interference), 혼잡(congestion), 인프라 장애에 따라 달라질 수 있다. 지속적인 연결을 전제로 자율성을 설계하면 취약한 의존성이 발생한다. 반면 로컬 지능을 중심으로 설계하고 외부 지원을 선택적으로 활용하면 운용 능력을 갑작스럽게 상실하는 대신 점진적 성능 저하(graceful degradation)가 가능한 시스템을 구축할 수 있다.

엣지 지능은 로봇 외부로 전송되는 정보량도 감소시킬 수 있다. 지속적인 원시 비디오(raw video), 포인트 클라우드(point cloud), 텔레메트리(telemetry)를 업로드하는 대신 온보드 소프트웨어는 중요한 이벤트를 식별하고, 특징(feature)을 추출하며, 관측 데이터를 압축하고, 요약 정보를 생성하거나 향후 학습을 위해 어려운 사례(difficult example)를 선택할 수 있다. 이는 네트워크와 저장장치 요구량을 줄이는 동시에 민감한 환경에서 요구되는 개인정보 보호(privacy), 보안(security), 데이터 주권(data sovereignty)을 지원한다.

엣지 지능과 외부 지능(external intelligence)의 경계는 고정되어 있기보다 동적으로 구성될 수 있어야 한다. 자주 사용되고 지연시간에 민감한 모델은 자연스럽게 온보드에 배치되는 반면, 계산 비용이 높고 사용 빈도가 낮은 작업은 가까운 인프라 또는 원격 인프라에서 실행할 수 있다. 모델 압축(model compression), 양자화(quantization), 지식 증류(distillation), 가지치기(pruning), 캐싱(caching), 적응형 추론(adaptive inference), 가속기 인지 최적화(accelerator-aware optimization)를 통해 하드웨어 크기와 전력 소비를 단순히 증가시키지 않고도 점점 더 높은 수준의 지능을 로봇에 탑재할 수 있다.

따라서 로봇 탑재 엣지 지능은 단순히 로봇 내부에 GPU를 설치하는 것을 의미하지 않는다. 이는 물리적 상호작용의 시간적 요구사항(timing requirement)을 중심으로 센싱(sensing), 컴퓨팅(computing), 메모리(memory), 인공지능 모델(AI model), 계획, 제어 인터페이스(control interface), 전력, 열 관리, 안전, 통신을 통합적으로 설계하는 것이다. 그 목적은 항상 접근 가능하다고 보장할 수 없는 외부 자원에 의존하지 않고도 로봇이 주변을 인식하고, 이해하고, 판단하고, 안전하게 행동하는 데 충분한 로컬 지능을 확보하는 것이다.

더 넓은 엣지--온프레미스--클라우드(edge--on-premise--cloud) 계층 구조에서 로봇 엣지(robot edge)는 자율성의 운용 중심(operational center)이 된다. 저수준 컴퓨팅은 결정론적인 물리 제어를 보장하고, 엣지는 센서 정보를 지능적인 행동으로 변환한다. 온프레미스 및 클라우드 시스템은 더 큰 모델, 플릿 협업(fleet coordination), 학습(training), 축적된 지식(accumulated knowledge)을 제공할 수 있지만 현실 세계와의 즉각적인 상호작용은 엣지가 담당한다. 이러한 특성으로 인해 로봇 탑재 엣지 지능은 확장 가능하고(scalable) 복원력 있는(resilient) 물리 인공지능(Physical AI)을 구현하기 위한 핵심 기반 능력이 된다.

## 05.03. On Premise AI Infrastructure

![](images/image3.png){width="7.268055555555556in" height="7.268055555555556in"}

온프레미스 인공지능 인프라(On-Premise AI Infrastructure)는 개별 로봇과 원격 클라우드 시스템(remote cloud system) 사이에 계산 계층(computational layer)을 제공하여, 물리 인공지능(Physical AI) 작업을 실제 운용 환경 가까이에 유지하면서 로봇에 탑재된 컴퓨팅의 연산 능력, 메모리, 저장공간, 열 관리 한계를 넘어설 수 있도록 한다. 공장, 물류창고, 병원, 캠퍼스, 보안 시설에서 이 계층은 여러 로봇을 동시에 지원하고, 즉각적인 자율성을 원격 인프라에 의존시키지 않으면서 공유 지능(shared intelligence)을 제공할 수 있다.

온프레미스 인프라의 주요 장점은 계산 근접성(computational proximity)이다. 로봇은 온보드 엣지 컴퓨터(onboard edge computer)가 제공할 수 있는 것보다 높은 처리 능력을 필요로 할 수 있지만, 모든 고부하 작업을 원격 클라우드로 전송하면 가변적인 지연시간(variable latency), 대역폭 제한, 서비스 의존성(service dependency), 연결성 위험(connectivity risk)이 발생할 수 있다. 로컬 GPU 서버 또는 인공지능 클러스터(AI cluster)는 통제된 로컬 네트워크를 통해 비교적 예측 가능한 통신 특성을 유지하면서 훨씬 더 많은 계산 자원을 제공한다.

이러한 아키텍처는 로봇 탑재 엣지 지능(on-robot edge intelligence)을 대체하지 않는다. 즉각적인 인식(perception), 충돌 회피(collision avoidance), 위치 추정(localization), 로컬 계획(local planning), 안전 모니터링(safety monitoring)과 같은 지연시간에 민감한 기능은 시간 요구사항에 따라 가능한 한 온보드에서 유지되어야 한다. 온프레미스 시스템은 계산 비용이 높지만 더 큰 지연시간을 허용할 수 있는 작업을 처리함으로써 로봇을 보완하며, 즉각적인 물리적 자율성(immediate physical autonomy)과 보다 심층적인 공유 지능 사이에 실용적인 역할 분담을 형성한다.

고부하 인공지능 추론(heavy AI inference)은 중요한 온프레미스 작업이다. 대규모 비전 모델(vision model), 멀티모달 모델(multimodal model), 월드 모델(world model), 파운데이션 모델(foundation model), 복잡한 추론 시스템(reasoning system)은 모바일 하드웨어가 지속적으로 지원할 수 있는 수준보다 더 많은 가속기 메모리(accelerator memory) 또는 계산 처리량(compute throughput)을 요구할 수 있다. 로봇은 소형 모델을 로컬에서 실행하면서 필요할 때 선택적으로 더 강력한 추론을 요청함으로써 모든 로봇에 고전력 하드웨어를 중복 탑재하지 않고 고성능 지능을 공유할 수 있다.

월드 모델링(world modeling)도 이러한 추가적인 계산 능력의 이점을 활용할 수 있다. 개별 로봇은 즉각적인 운용에 필요한 로컬 표현(local representation)을 유지하고, 온프레미스 인프라는 여러 로봇과 더 긴 시간 범위에서 수집된 관측 정보를 통합할 수 있다. 이를 통해 더 넓은 공간 지도(spatial map), 풍부한 의미론적 표현(semantic representation), 과거 상태 정보(historical state information), 공유 점유 정보(shared occupancy), 주행 가능성 지식(traversability knowledge), 그리고 개별 로봇의 단기 세계 이해를 넘어서는 고부하 예측 모델(prediction model)을 운영할 수 있다.

플릿 협업(fleet coordination)은 이러한 공유 계층에 자연스럽게 배치된다. 동일한 환경에서 여러 로봇이 운용될 때 완전히 독립적인 계획만 수행하면 혼잡(congestion), 비효율적인 경로, 중복 탐색, 자원 충돌, 충전소와 작업 공간에 대한 경쟁이 발생할 수 있다. 온프레미스 조정기(on-premise coordinator)는 더 넓은 운용 상황을 유지하면서 작업 할당(task allocation), 교통 관리(traffic management), 경로 우선순위, 충전 일정(charging schedule), 공유 자원, 협력 행동(cooperative behavior)을 최적화할 수 있으며, 각 로봇은 즉각적인 로컬 안전을 계속 담당한다.

공유 지도(shared map)와 월드 메모리(world memory)는 또 다른 중요한 기능을 제공한다. 모든 로봇이 동일한 환경 변화를 독립적으로 다시 발견하는 대신 한 로봇이 수집한 관측을 검증하고 통합하여 다른 로봇에 배포할 수 있다. 차단된 통로, 변경된 선반 구성, 위험 지역, 임시 장애물, 새롭게 통과 가능해진 경로가 플릿 전체의 지식으로 전환될 수 있다. 온프레미스 인프라는 개별 로봇의 고립된 경험을 지속적으로 갱신되는 집단 운용 지식(collective operational knowledge)으로 변환한다.

데이터 통합(data aggregation) 역시 중요하다. 물리 인공지능 시스템은 대량의 센서 측정값, 이벤트(event), 로그(log), 모델 출력(model output), 운용 텔레메트리(operational telemetry)를 생성한다. 모든 정보를 원격 클라우드 저장소로 직접 업로드하면 상당한 외부 대역폭을 소비하며 반드시 필요한 것도 아니다. 로컬 인프라는 선택된 로봇 데이터를 수신하여 정리하고, 중복을 제거하고, 압축하고, 중요한 이벤트를 색인화(indexing)하며, 어떤 정보를 로컬에 유지하고 어떤 정보를 장기 저장 및 학습을 위해 외부로 전송할지 결정할 수 있다.

이러한 로컬 데이터 계층(local data layer)은 물리 인공지능 데이터 엔진(Physical AI data engine)을 지원할 수 있다. 로봇은 어려운 상황, 예측 실패(prediction failure), 개입(intervention), 비정상적인 센서 관측, 낮은 신뢰도의 의사결정을 식별하여 온프레미스 시스템에 업로드할 수 있다. 이러한 샘플은 학습 및 평가 데이터셋으로 큐레이션(curation)되고, 디버깅을 위해 재생되며, 반복되는 실패 모드(failure mode)를 분석하고, 궁극적으로 모델 개선 파이프라인(model improvement pipeline)에 통합될 수 있다. 이를 통해 운용 경험이 재사용 가능한 학습 자원으로 전환된다.

모델 관리(model management) 역시 로컬에서 중앙집중화할 수 있다. 모든 로봇을 개별적으로 설정하는 대신 온프레미스 인프라는 승인된 모델 버전(model version), 배포 정책(deployment policy), 호환성 정보, 설정 파일(configuration file), 롤백 패키지(rollback package)를 관리할 수 있다. 새로운 모델은 플릿에 배포하기 전에 평가할 수 있으며 선택된 로봇부터 단계적으로 배포할 수 있다. 성능이 저하되면 모든 로봇이 외부 클라우드 서비스와 직접 통신하지 않고도 이전 버전으로 복원할 수 있다.

시뮬레이션(simulation)과 검증(validation) 작업도 로컬 고성능 인프라에서 실행할 수 있다. 기록된 시나리오를 후보 인식, 계획, 월드 모델 구성요소에 대해 재생하여 배포 전에 검증할 수 있다. 디지털 환경(digital environment)에서는 다양한 매개변수, 정책, 모델 버전을 시험할 수 있으며, 하드웨어 인 더 루프(hardware-in-the-loop) 또는 소프트웨어 인 더 루프(software-in-the-loop)를 통해 실제 운용 조건을 재현할 수 있다. 이를 통해 인공지능 개발과 실제 물리 시스템 배포 사이에 로컬 검증 단계가 형성된다.

로봇과 온프레미스 인프라를 연결하는 네트워크(network)는 아키텍처의 핵심 구성요소가 된다. 이더넷(Ethernet), 산업용 네트워크(industrial network), 사설 와이파이(private Wi-Fi), 사설 셀룰러 네트워크(private cellular system)는 서로 다른 대역폭, 지연시간, 결정성(determinism), 이동성(mobility), 신뢰성 조합을 제공할 수 있다. 따라서 시간 동기화(time synchronization), 메시지 우선순위(message prioritization), 혼잡 관리(congestion management), 인증(authentication), 네트워크 이중화(network redundancy)는 인공지능 작업 배치와 함께 고려되어야 한다.

고품질 로컬 네트워크를 구축하더라도 로봇은 온프레미스 서비스가 항상 사용 가능하다고 가정해서는 안 된다. 서버가 고장 나거나 스위치가 전원을 잃을 수 있으며, 무선 통신 범위가 악화되거나 유지보수로 인프라가 일시적으로 중단될 수 있다. 따라서 필수적인 자율 행동은 온보드에서 계속 실행할 수 있어야 한다. 연결이 끊어지면 로봇은 적절한 성능 저하 모드(degraded mode)에서 운용을 지속하고, 연결이 복구되면 축적된 데이터를 동기화하거나 상위 서비스를 다시 요청해야 한다.

온프레미스 인프라는 자동으로 데이터를 외부 서비스에 전송하는 아키텍처보다 민감한 정보에 대한 더 강력한 운용 통제(operational control)를 제공할 수 있다. 카메라 영상, 시설 지도, 생산 정보, 인간 상호작용 데이터, 독점적인 로봇 텔레메트리(proprietary robot telemetry)를 조직이 통제하는 환경 내부에 유지할 수 있다. 이는 개인정보 보호(privacy), 보안(security), 지식재산권(intellectual property), 규제 요구사항(regulatory requirement), 데이터 주권(data sovereignty)이 운용 정보의 처리 및 저장 위치를 제한하는 경우 특히 중요하다.

그러나 공유 인프라는 플릿 전체에 영향을 줄 수 있는 높은 가치의 공격 대상이자 잠재적인 전파 지점이 될 수 있으므로 보안은 명시적으로 설계되어야 한다. 로봇 식별(robot identity), 인증, 암호화 통신(encrypted communication), 접근 제어(access control), 네트워크 분할(network segmentation), 모델 무결성 검증(model integrity verification), 안전한 업데이트, 로깅(logging), 이상 탐지(anomaly monitoring)를 통해 손상된 장치나 서비스가 다른 시스템에 영향을 미치는 것을 방지해야 한다. 안전 중요 로봇 기능은 인프라 장애나 승인되지 않은 명령으로부터 격리되어야 한다.

플릿 규모가 증가하면 자원 스케줄링(resource scheduling)이 중요해진다. 여러 로봇이 동시에 GPU 추론, 지도 처리, 최적화, 저장, 시뮬레이션, 데이터 분석을 요청할 수 있다. 따라서 인프라 용량은 작업 우선순위(workload priority), 허용 지연시간(latency tolerance), 안전 관련성, 사용 가능한 가속기 자원에 따라 할당되어야 한다. 실시간 운용 요청은 오프라인 분석(offline analytics)보다 높은 우선순위를 받을 수 있으며, 배치 처리(batch processing)와 학습은 계산 수요가 감소할 때까지 연기할 수 있다.

확장성(scalability)을 위해 반드시 하나의 거대한 서버가 필요한 것은 아니다. 온프레미스 인프라는 하나의 GPU 워크스테이션(workstation)에서 시작하여 여러 추론 서버(inference server), 저장 노드(storage node), 오케스트레이션 서비스(orchestration service), 가속기 클러스터(accelerator cluster)로 확장할 수 있다. 작업 특성에 따라 부하를 분산하고 로봇 수 또는 모델 복잡도가 증가함에 따라 자원을 추가할 수 있다. 이를 통해 각 로봇의 하드웨어 구성을 변경하지 않고도 물리 인공지능 시스템의 계산 능력을 독립적으로 확장할 수 있다.

전력 및 열 관리(power and thermal management) 측면에서도 이러한 분리가 유리하다. 고성능 가속기는 수백 와트의 전력을 소비하고 상당한 냉각 능력을 요구할 수 있어 배터리 기반 모바일 플랫폼에서 지속적으로 운용하기 어렵다. 고정형 인프라는 더 높은 전력 공급 능력, 대형 냉각 시스템, 더 많은 메모리, 고밀도 가속기 구성을 제공할 수 있다. 적절한 작업을 로봇 외부로 이동시키면 온보드 운용 시간을 향상시키면서 로봇 자체에서는 물리적으로 구현하기 어려운 인공지능 기능을 제공할 수 있다.

온프레미스 인프라는 운용 기술(operational technology)과 글로벌 클라우드 서비스(global cloud service) 사이에 유용한 경계를 형성하기도 한다. 외부 인터넷 연결이 중단되어도 로컬 로봇과 서버는 시설의 운용을 지속할 수 있으며, 클라우드 시스템은 대규모 학습, 사이트 간 분석(cross-site analytics), 장기 저장, 소프트웨어 생애주기 관리(software lifecycle management), 조직 전체 학습을 담당할 수 있다. 모든 운용 프로세스를 클라우드에 직접 의존시키는 대신 필요한 정보만 선택적으로 상위 계층으로 이동시킬 수 있다.

다중 사이트 배포(multi-site deployment)에서는 각 시설이 자체적인 로컬 지능을 유지하면서 선택된 지식만 전역적으로 통합할 수 있다. 사이트별 지도, 운용 제약조건, 민감한 데이터는 온프레미스에 유지하고, 일반화된 모델 개선 결과, 익명화된 통계(anonymized statistics), 검증된 학습 결과물(learning artifact)은 클라우드 인프라로 전달할 수 있다. 개선된 모델은 이후 계층 구조를 통해 다시 내려와 로컬에서 검증된 뒤 배포될 수 있으며, 이를 통해 여러 사이트와 플릿을 연결하는 통제된 학습 루프(controlled learning loop)가 형성된다.

따라서 적절한 온프레미스 인프라 규모는 로봇 수, 센서 데이터 양, 모델 복잡도, 플릿 협업 요구사항, 데이터 보존 기간(storage retention), 허용 가능한 지연시간, 예상 최대 작업 부하(peak workload)에 따라 결정되어야 한다. 소규모 배포에서는 하나의 로컬 서버만으로 충분할 수 있지만 대규모 물리 인공지능 플릿에서는 여러 GPU 노드, 고속 네트워크, 대용량 저장장치, 이중화(redundancy), 오케스트레이션, 모니터링이 필요할 수 있다. 인프라 규모 산정(infrastructure sizing)은 단순히 사용 가능한 계산 능력을 최대화하기보다 실제 운용 요구사항을 기반으로 이루어져야 한다.

궁극적으로 온프레미스 인공지능 인프라는 로봇의 유일한 지능 공급원이 되지 않으면서 로컬 물리 인공지능 생태계(Physical AI ecosystem)의 공유 계산 두뇌(shared computational brain) 역할을 한다. 로봇은 엣지에서 즉각적인 자율 능력을 유지하고, 로컬 인프라는 고부하 추론, 공유 월드 지식(shared world knowledge), 플릿 협업, 데이터 관리, 모델 서비스를 제공하며, 클라우드 자원은 더 광범위한 학습과 확장성을 담당한다. 이러한 계층적 관계는 지연시간 제어, 복원력(resilience), 보안, 확장 가능한 플릿 운용을 유지하면서 고성능 물리 인공지능을 구현할 수 있도록 한다.

## 05.04. Cloud AI Infrastructure

![](images/image4.png){width="7.268055555555556in" height="7.268055555555556in"}

클라우드 인공지능 인프라(Cloud AI Infrastructure)는 계층형 물리 인공지능 아키텍처(hierarchical Physical AI architecture)에서 가장 높은 수준의 계산 용량을 제공하는 계층이다. 즉각적인 물리적 상호작용을 우선하는 로봇 탑재 엣지 컴퓨팅(on-robot edge computing)이나 로컬 플릿 지능(local fleet intelligence)을 지원하는 온프레미스 인프라(on-premise infrastructure)와 달리, 클라우드는 대규모 연산, 대용량 저장공간, 탄력적 확장(elastic scaling), 장시간 처리(long processing horizon)가 필요한 작업에 최적화된다. 이를 통해 개별 로봇과 시설이 가진 한계를 넘어 지능을 확장할 수 있다.

클라우드는 계산 수요가 시간에 따라 크게 변하는 경우 특히 유용하다. 파운데이션 모델(foundation model)을 학습하거나 새롭게 수집된 플릿 데이터셋(fleet dataset)을 처리하거나 수천 개의 시뮬레이션 실험을 실행할 때 일시적으로 수백 또는 수천 개의 가속기 인스턴스(accelerator instance)가 필요할 수 있다. 모든 배포 사이트에 동일한 자원을 영구적으로 유지하는 것은 비효율적이다. 탄력적 클라우드 인프라(elastic cloud infrastructure)는 고부하 작업이 발생할 때 컴퓨팅, 메모리, 저장 용량을 확장하고 작업이 종료되면 다시 축소할 수 있도록 한다.

대규모 모델 학습(large-scale model training)은 물리 인공지능에서 가장 중요한 클라우드 작업 중 하나이다. 인식 네트워크(perception network), 멀티모달 모델(multimodal model), 월드 모델(world model), 비전-언어-행동 모델(vision-language-action model), 범용 로봇 정책(general robot policy)은 방대한 데이터셋과 분산 가속기 클러스터(distributed accelerator cluster)를 요구할 수 있다. 클라우드 인프라는 다중 GPU 및 다중 노드 학습(multi-node training)을 조정하면서 대규모 모델에 필요한 저장 처리량, 체크포인트 관리(checkpoint management), 실험 추적(experiment tracking), 분산 통신(distributed communication)을 제공할 수 있다.

물리 인공지능은 클라우드 규모의 데이터 통합(cloud-scale data aggregation)을 통해서도 큰 이점을 얻을 수 있다. 개별 로봇은 서로 다른 환경, 객체, 작업, 실패, 개입(intervention), 운용 조건을 나타내는 관측 데이터를 생성한다. 선택된 정보는 로봇에서 온프레미스 시스템을 거쳐 중앙 데이터 플랫폼(centralized data platform)으로 이동할 수 있다. 여러 플릿과 사이트의 경험을 통합하면 개별 로봇이 단독으로 획득할 수 있는 것보다 훨씬 다양한 데이터셋을 구축할 수 있으며, 이를 통해 더 광범위한 일반화(generalization)와 지속적인 모델 개선을 지원할 수 있다.

따라서 클라우드는 물리 인공지능 데이터 엔진(Physical AI data engine)의 상위 계층이 될 수 있다. 운용 데이터는 색인화(indexing), 필터링(filtering), 중복 제거(deduplication), 분류(categorization) 과정을 거쳐 학습과 평가에 재사용할 수 있는 데이터셋으로 변환될 수 있다. 어려운 시나리오, 모델 실패, 비정상적인 환경 조건, 인간 개입(human intervention), 낮은 신뢰도의 예측에는 특별한 우선순위를 부여할 수 있다. 목표는 단순히 데이터를 축적하는 것이 아니라 분산된 물리적 경험을 점점 더 가치 있는 학습 자료로 전환하는 것이다.

시뮬레이션(simulation)은 클라우드 규모의 컴퓨팅으로부터 자연스럽게 이점을 얻는 또 다른 작업이다. 다수의 가상 로봇(virtual robot)이 서로 다른 환경, 객체 구성, 조명 조건, 센서 특성, 물리 매개변수, 고장 조건에서 시나리오를 병렬로 실행할 수 있다. 이러한 대규모 병렬 시뮬레이션(massive parallel simulation)은 합성 데이터(synthetic data)를 생성하고, 정책을 평가하며, 도메인 랜덤화(domain randomization)를 수행하고, 희귀 사건(rare event)을 탐색하며, 실제 물리 시스템에서 비용이 크거나 위험한 실험을 수행하기 전에 후보 모델을 시험할 수 있게 한다.

클라우드 인프라는 실시간 제어 루프(real-time control loop)에 적합하지 않은 장기적이고 계산 집약적인 추론도 지원할 수 있다. 대규모 파운데이션 모델, 복잡한 계획 알고리즘(planning algorithm), 전역 최적화(global optimization), 광범위한 탐색(search), 다단계 추론(multi-stage reasoning)은 지속적인 온보드 실행에 부적합한 수준의 자원을 사용할 수 있다. 이러한 시스템은 전략적 권고, 작업 분해(task decomposition), 지식 검색(knowledge retrieval), 계획 지원을 제공하는 반면, 엣지 시스템은 즉각적인 물리적 의사결정과 안전을 계속 담당한다.

플릿 전체 분석(fleet-wide analytics)은 클라우드의 또 다른 중요한 역할이다. 클라우드 서비스는 수천 대의 로봇과 여러 사이트의 성능을 비교하여 반복되는 고장, 에너지 사용 추세, 부품 열화(component degradation), 위치 추정 문제, 인식 취약점, 임무 효율성(mission efficiency), 비정상적인 운용 패턴을 식별할 수 있다. 개별적으로는 중요하지 않아 보이는 로컬 이벤트도 대규모 플릿 전체에서 분석하면 통계적으로 의미 있는 패턴이 되어 체계적인 엔지니어링 및 운용 개선으로 이어질 수 있다.

클라우드는 지리적으로 분리된 배포 환경 사이의 지식 공유(knowledge sharing)도 가능하게 한다. 한 시설에서 동작하는 로봇이 다른 사이트에도 유용한 행동, 어려운 시나리오, 모델 취약점을 발견할 수 있다. 개별 사이트가 직접 정보를 교환하는 대신 검증된 지식을 중앙에서 통합하고 학습을 통해 일반화한 뒤 개선된 모델 또는 정책(policy)으로 다시 배포할 수 있다. 이를 통해 여러 물리 환경을 연결하는 플릿 규모 학습 루프(fleet-scale learning loop)가 형성된다.

모델 생애주기 관리(model lifecycle management) 역시 이러한 상위 계층을 통해 조정할 수 있다. 학습 결과물(training artifact), 모델 버전, 평가 결과, 배포 메타데이터(deployment metadata), 호환성 정보, 과거 설정을 중앙에서 관리할 수 있다. 후보 모델은 온프레미스 인프라에 배포되기 전에 검증 단계를 통과할 수 있으며, 이후 사이트별 추가 시험을 수행할 수 있다. 승인된 모델은 롤백 경로(rollback path)를 유지한 상태에서 로봇에 단계적으로 배포하여 예상하지 못한 동작이 발생할 경우 이전 버전으로 복원할 수 있다.

소프트웨어 생애주기 기능(software lifecycle function)도 유사한 계층 구조를 따를 수 있다. 클라우드 서비스는 대규모 로봇 집단을 위한 소프트웨어 패키지, 컨테이너(container), 설정 기준(configuration baseline), 보안 업데이트, 배포 매니페스트(deployment manifest)를 관리할 수 있다. 모든 로봇에 클라우드에서 직접 업데이트를 배포할 필요는 없다. 먼저 통제된 온프레미스 시스템으로 전달한 후 로컬에서 단계적으로 배포하면 외부 대역폭 소비를 줄이고 시설 운영자가 실제 운용 장비의 업데이트 시점을 결정할 수 있다.

장기 저장(long-term storage)은 물리 인공지능 프로그램이 수년에 걸쳐 막대한 데이터를 축적할 수 있기 때문에 클라우드 인프라에 특히 적합하다. 원시 센서 기록(raw sensor recording), 선택된 이벤트 시퀀스(event sequence), 지도, 로그, 모델 체크포인트(model checkpoint), 시뮬레이션 결과, 평가 데이터셋, 엔지니어링 결과물은 로봇과 개별 시설의 실용적인 저장 용량을 초과할 수 있다. 계층형 클라우드 저장소(tiered cloud storage)는 가치가 높은 과거 정보를 보존하고 가치가 낮은 데이터는 요약, 압축, 아카이빙(archiving), 삭제할 수 있도록 한다.

그러나 클라우드 컴퓨팅을 물리적 기계의 즉각적인 제어 중심(immediate control center)으로 간주해서는 안 된다. 인터넷 경로에는 가변적인 지연시간, 지터(jitter), 혼잡, 라우팅 변화, 통신 장애가 발생할 수 있다. 수백 밀리초 또는 수 초 늦게 도착한 명령은 이미 의미가 없거나 위험할 수 있다. 따라서 충돌 회피, 안정화(stabilization), 비상 대응(emergency response), 기타 시간 중요 기능(time-critical function)은 광역 네트워크 연결에 의존하지 않는 하위 계층에서 수행되어야 한다.

이러한 분리는 중요한 아키텍처 원칙을 형성한다. 클라우드 지능(cloud intelligence)은 자율성을 강화해야 하지만 기본적인 자율성을 가능하게 하는 필수조건이 되어서는 안 된다. 클라우드 연결이 끊어져도 로봇은 필수적인 운용을 지속해야 하며, 가능한 경우 온프레미스 인프라가 로컬 공유 서비스를 유지해야 한다. 외부 연결이 복구되면 버퍼링된 데이터(buffered data)를 동기화하고 새로운 모델을 가져오며 클라우드 지원 기능을 재개할 수 있지만, 이를 위해 물리 시스템의 기본적인 자율 기능을 다시 시작할 필요가 없어야 한다.

대역폭(bandwidth)은 모든 로봇 데이터를 클라우드로 직접 전송하지 않아야 하는 또 다른 이유이다. 여러 고해상도 카메라, 라이다, 레이더 시스템, 오디오 장치, 내부 텔레메트리는 매우 큰 연속 데이터 스트림을 생성할 수 있다. 엣지 및 온프레미스 시스템은 이벤트 선택(event selection), 압축, 특징 추출(feature extraction), 요약, 데이터 큐레이션(data curation)을 통해 이러한 정보를 축소해야 한다. 클라우드 인프라는 전송 및 저장 비용을 정당화할 만큼 장기적인 학습 또는 조직적 가치가 있는 정보를 중심으로 수신해야 한다.

물리 인공지능 시스템이 실제 운용 환경과 글로벌 인프라를 연결하면서 보안(security)은 특히 중요해진다. 장치 식별(device identity), 암호화 통신(encrypted communication), 인증(authentication), 접근 제어(access control), 안전한 API, 키 관리(key management), 모델 서명(model signing), 감사 로깅(audit logging), 소프트웨어 공급망 보호(software supply-chain protection)를 통해 승인되지 않은 접근이나 악의적인 업데이트를 방지해야 한다. 클라우드가 잘못되거나 위험한 명령으로부터 로봇을 보호하는 독립적인 안전 메커니즘을 우회할 수 있어서는 안 된다.

개인정보 보호(privacy)와 데이터 주권(data sovereignty) 역시 클라우드 사용을 제한할 수 있다. 사람의 이미지, 산업 공정, 시설 지도, 의료 정보, 독점적인 운용 데이터는 특정 로봇, 시설, 조직 또는 관할권(jurisdiction) 밖으로 이동하는 것이 금지될 수 있다. 계층형 아키텍처는 민감한 정보를 엣지 또는 온프레미스에 유지하면서 익명화된 통계(anonymized statistics), 학습된 표현(learned representation), 승인된 데이터셋, 모델 업데이트만 클라우드 서비스로 이동하도록 한다. 따라서 클라우드 도입이 무제한적인 데이터 중앙집중화(data centralization)를 의미하지는 않는다.

다중 리전 및 다중 사이트 아키텍처(multi-region and multi-site architecture)는 가용성과 조직적 확장성을 더욱 향상시킬 수 있다. 각 시설은 로컬 인프라를 통해 독립적으로 운용하면서 클라우드 서비스는 글로벌 모델 저장소(global model repository), 사이트 간 분석(cross-site analytics), 공유 학습 파이프라인(shared learning pipeline), 재해 복구 자원(disaster recovery resource)을 유지할 수 있다. 하나의 리전이나 서비스에 장애가 발생하더라도 로컬 자율성은 계속 유지될 수 있다. 따라서 글로벌 지능의 가용성과 물리적 운용에 필요한 즉각적인 가용성을 분리할 수 있다.

탄력적 인프라는 작업 부하를 적절히 통제하지 않으면 대형 가속기, 고성능 저장장치, 지속적인 데이터 전송, 상시 서비스 사용으로 높은 비용이 발생할 수 있으므로 비용 관리(cost management)는 중요한 설계 요소이다. 학습, 시뮬레이션, 추론, 분석, 저장 작업을 가치와 긴급성에 따라 분류해야 한다. 배치 작업(batch workload)의 스케줄링, 적절한 가속기 유형 선택, 데이터 보존 정책(retention policy) 관리, 불필요한 데이터 이동 최소화를 통해 계산 경제성(computational economics)을 크게 향상시킬 수 있다.

클라우드, 온프레미스, 엣지 컴퓨팅 사이의 경계는 하드웨어와 모델이 발전함에 따라 적응 가능해야 한다. 처음에는 클라우드 GPU가 필요했던 모델도 양자화(quantization), 지식 증류(distillation), 가지치기(pruning), 하드웨어 성능 향상을 통해 이후 로컬 서버나 로봇 가속기에서 실행할 수 있을 정도로 작아질 수 있다. 반대로 더욱 강력한 파운데이션 모델은 새로운 작업을 상위 계층으로 이동시킬 수도 있다. 따라서 계층형 물리 인공지능은 알고리즘을 특정 위치에 영구적으로 고정하기보다 작업 이동(workload migration)을 지원해야 한다.

궁극적으로 클라우드 인공지능 인프라는 개별 로봇이 경제적으로 자체 유지하기 어려운 장기 학습(long-term learning), 대규모 계산(large-scale computation), 글로벌 지식(global knowledge), 생애주기 관리 기능(lifecycle capability)을 제공한다. 엣지 시스템은 즉각적인 자율성을 유지하고, 온프레미스 시스템은 로컬 지능을 조정하며, 클라우드 시스템은 여러 플릿과 사이트 그리고 장기간의 운용 경험으로부터 학습한다. 이러한 계층들이 결합됨으로써 고립된 개별 로봇은 지속적으로 개선되는 물리 인공지능 생태계(Physical AI ecosystem)에 참여하는 지능형 시스템으로 발전할 수 있다.

## 05.05. Edge On Premise Cloud Role Definition

![](images/image5.png){width="7.268055555555556in" height="7.268055555555556in"}

엣지--온프레미스--클라우드 아키텍처(edge--on-premise--cloud architecture)는 물리 인공지능(Physical AI)의 작업을 물리 세계(physical world)와의 관계에 따라 어디에서 실행해야 하는지를 정의한다. 목표는 하나의 컴퓨팅 위치를 모든 상황에서 우월한 것으로 지정하는 것이 아니라, 각 기능의 지연시간(latency), 안전성(safety), 연산 능력(compute), 저장공간(storage), 연결성(connectivity), 확장성(scalability) 요구사항을 만족할 수 있는 위치에 배치하는 것이다. 세 계층은 함께 즉각적인 물리적 상호작용에서 플릿 지능(fleet intelligence)과 글로벌 학습(global learning)까지 이어지는 연속적인 컴퓨팅 구조를 형성한다.

엣지(edge)는 로봇의 즉각적인 자율성(immediate robot autonomy)을 담당하는 운용 중심(operational center)이다. 짧은 시간 범위에서 물리적 행동에 직접 영향을 미치는 기능은 센서와 액추에이터 가까이에 유지되어야 한다. 인식(perception), 위치 추정(localization), 상태 추정(state estimation), 장애물 회피(obstacle avoidance), 로컬 계획(local planning), 단기 예측(short-horizon prediction), 안전 모니터링(safety monitoring), 즉각적인 의사결정(immediate decision making)은 낮은 지연시간과 네트워크 중단 상황에서도 지속적인 가용성이 필요하기 때문에 일반적으로 이 계층에 속한다.

따라서 엣지 컴퓨팅(edge computing)은 외부 인프라와 독립적으로 최소 자율 능력(minimum autonomous capability)을 유지해야 한다. 로봇은 온프레미스 또는 클라우드 서비스를 사용할 수 없더라도 관련 위험 요소를 인식하고, 자신의 상태를 추정하며, 안전한 로컬 행동을 생성하고, 적절한 폴백 모드(fallback mode)로 진입할 수 있어야 한다. 외부 지능(external intelligence)은 성능을 향상시킬 수 있지만 연결이 끊어진다고 해서 로봇의 안전한 운용 능력이 즉시 사라져서는 안 된다.

엣지는 또한 고대역폭 원시 센서 스트림(high-bandwidth raw sensor stream)을 유용한 표현으로 변환하는 역할을 담당한다. 카메라(camera), 라이다(LiDAR), 레이더(radar), 관성측정장치(IMU), 엔코더(encoder), 기타 센서는 로봇 외부로 지속적으로 전송하기에는 지나치게 많은 정보를 생성할 수 있다. 로컬 처리를 통해 객체(object), 점유 상태(occupancy), 궤적(trajectory), 의미론적 특징(semantic feature), 이벤트(event), 신뢰도 추정(confidence estimate), 압축된 관측(compressed observation)을 생성함으로써 상위 계층에 필요한 정보를 유지하면서 통신 요구량을 줄일 수 있다.

온프레미스 인프라(on-premise infrastructure)는 개별 로봇의 자율성과 글로벌 컴퓨팅(global computation) 사이의 중간 계층을 차지한다. 이 계층의 역할은 동일한 시설, 캠퍼스, 물류창고, 공장, 병원, 보안 환경에서 운용되는 로봇들에게 공유 지능(shared intelligence)을 제공하는 것이다. 개별 로봇보다 더 높은 연산 및 저장 용량을 제공하면서 원격 클라우드 서비스보다 낮고 통제 가능한 통신 지연시간을 유지할 수 있다.

운용상 유용하지만 온보드 실행이 필요할 정도로 시간에 민감하지 않은 고부하 추론(heavy inference)은 이러한 중간 계층에 배치할 수 있다. 더 큰 월드 모델(world model), 멀티모달 모델(multimodal model), 의미론적 추론 시스템(semantic reasoning system), 지도 처리(map processing), 시나리오 분석(scenario analysis), 최적화 서비스(optimization service)를 로컬 GPU 서버에서 실행할 수 있다. 로봇은 필수적인 제어와 즉각적인 자율성을 로컬에서 유지하면서 이러한 기능을 선택적으로 요청할 수 있다.

플릿 협업(fleet coordination)은 개별 로봇의 관점을 넘어서는 정보가 필요하기 때문에 온프레미스 인프라에 특히 적합하다. 작업 할당(task allocation), 교통 조정(traffic coordination), 공유 자원 관리(shared resource management), 충전 일정(charging schedule), 경로 우선순위(route priority), 협력 임무(cooperative mission), 플릿 수준 최적화(fleet-level optimization)는 여러 기계의 상태에 대한 지식을 필요로 한다. 이러한 넓은 운용 상황을 로컬에서 중앙집중적으로 관리하면 모든 의사결정을 원격 클라우드로 이동시키지 않고도 협력 행동을 구현할 수 있다.

공유 월드 지식(shared world knowledge) 역시 온프레미스 계층에 자연스럽게 위치한다. 여러 로봇의 관측을 통합하여 시설 수준 지도(facility-level map), 의미론적 표현(semantic representation), 점유 정보(occupancy information), 주행 가능성 지식(traversability knowledge), 운용 메모리(operational memory)를 구축할 수 있다. 한 로봇이 차단된 경로나 환경 변화를 발견하면 검증된 정보를 다른 로봇에 배포함으로써 플릿 전체가 동일한 상황을 반복적으로 다시 발견하지 않고 집단 경험(collective experience)을 활용할 수 있다.

클라우드(cloud)는 가장 상위 수준의 계산 및 학습 계층(computational and learning layer)을 제공한다. 주요 역할은 즉각적인 로봇 제어가 아니라 대규모 학습(large-scale training), 시뮬레이션(simulation), 장기 저장(long-term storage), 플릿 전체 분석(fleet-wide analytics), 파운데이션 모델 개발(foundation-model development), 전역 최적화(global optimization), 생애주기 관리(lifecycle management)이다. 이러한 작업은 막대한 자원을 사용할 수 있지만 물리적 상호작용과 직접 연결된 기능보다 훨씬 긴 처리 및 통신 시간을 허용할 수 있다.

클라우드 인프라는 여러 시설과 플릿의 경험을 결합하는 데 특히 유용하다. 선택된 데이터, 어려운 시나리오, 모델 실패(model failure), 개입 기록(intervention record), 운용 통계(operational statistics)를 전역적으로 통합할 수 있다. 학습 시스템은 이러한 분산된 경험을 개선된 인식 모델, 월드 모델, 정책(policy), 파운데이션 모델로 변환하고, 이후 검증 과정을 거쳐 계층 구조를 통해 다시 하위 시스템으로 배포할 수 있다.

이러한 구조는 자연스럽게 서로 다른 시간 척도(time scale)를 형성한다. 엣지 기능은 빠르게 변화하는 물리적 조건과 직접 상호작용하기 때문에 밀리초에서 수 초 범위에서 동작한다. 온프레미스 기능은 준실시간 협업(near-real-time coordination)에서 수 분 이상의 보다 넓은 운용 시간 범위에서 동작할 수 있다. 클라우드 기능은 학습, 시뮬레이션, 분석, 조직 전체 학습(organization-wide learning)을 수행하면서 수 시간, 수 일 또는 그 이상의 시간 범위에서 동작할 수 있다.

안전 중요도(safety criticality)는 역할을 정의하는 또 하나의 중요한 기준이다. 계산 기능이 즉각적인 물리적 피해를 발생시킬 가능성과 직접적으로 연결될수록 원격 통신이나 공유 인프라에 대한 의존성은 낮아야 한다. 따라서 안정화(stabilization), 비상 대응(emergency response), 충돌 회피, 보호 제어(protective control)는 로컬에 유지되어야 한다. 반면 상위 수준 최적화와 권고 기능은 결과가 지연되거나 일시적으로 제공되지 않더라도 즉각적인 물리적 안전을 반드시 손상시키는 것은 아니므로 상위 계층으로 이동할 수 있다.

연산 집약도(compute intensity)는 반대 방향의 압력을 발생시킨다. 경량이며 지연시간에 민감한 작업은 엣지 실행에 적합하고, 모델 규모와 계산 비용이 증가할수록 온프레미스 또는 클라우드 자원을 사용하는 것이 유리하다. 적절한 경계는 이러한 상반된 요구사항 사이의 균형을 통해 결정된다. 대규모 모델이라고 해서 결과가 즉시 필요할 경우 무조건 클라우드에서 실행해서는 안 되며, 작은 모델이라고 해서 사용 빈도가 낮은 경우 반드시 온보드 자원을 소비할 필요도 없다.

연결성 허용도(connectivity tolerance)는 작업 배치를 결정하는 또 다른 요소이다. 통신 장애에도 지속적으로 실행되어야 하는 기능은 온보드에 유지되어야 한다. 시설 네트워크의 일시적인 중단을 허용할 수 있는 기능은 온프레미스에서 실행할 수 있으며, 인터넷 연결을 기다릴 수 있는 작업은 클라우드에 배치할 수 있다. 이러한 의존성을 기준으로 시스템을 설계하면 하나의 통신 계층이 사라졌을 때 전체 시스템이 갑자기 실패하는 대신 점진적으로 성능이 저하(graceful degradation)되도록 할 수 있다.

데이터 지역성(data locality) 역시 중요하다. 원시 센서 스트림은 대역폭 또는 개인정보 보호 요구사항 때문에 엣지에 유지할 수 있으며, 선택된 이벤트와 통합된 운용 정보는 온프레미스 인프라로 이동할 수 있다. 더 광범위한 학습, 아카이빙(archiving), 조직적 가치가 있는 데이터만 클라우드까지 전달될 수 있다. 이러한 단계적인 축소를 통해 방대한 물리 세계의 데이터 스트림은 상위 계층으로 이동하면서 점차 압축되고 전역적으로 유용한 정보로 변환된다.

반대 방향으로는 원시 경험(raw experience)이 아니라 지능(intelligence)이 이동한다. 클라우드 시스템은 파운데이션 모델, 개선된 정책, 소프트웨어 릴리스(software release), 글로벌 지식을 생성할 수 있다. 온프레미스 인프라는 이러한 결과물을 특정 시설에 맞게 검증, 사용자 정의(customization), 캐싱(caching), 관리할 수 있다. 이후 엣지 시스템은 자체 하드웨어와 즉각적인 작업에 적합하도록 최적화된 모델, 지도, 설정(configuration), 정책을 전달받는다. 따라서 계층 구조는 양방향 데이터 및 모델 흐름(bidirectional data and model flow)을 지원한다.

모든 작업을 하나의 계층에 영구적으로 고정할 필요는 없다. 모델 압축(model compression), 양자화(quantization), 지식 증류(distillation), 새로운 가속기(accelerator), 네트워크 개선, 로봇 임무 변화, 안전 요구사항 변화에 따라 적절한 실행 위치가 달라질 수 있다. 처음에는 클라우드 자원이 필요했던 모델이 이후 온프레미스나 로봇으로 이동할 수 있으며, 새롭게 확장된 추론 모델은 상위 계층으로 이동할 수 있다. 따라서 역할 정의는 작업 이동성(workload mobility)을 지원해야 한다.

일부 기능은 여러 계층에 걸쳐 분할할 수도 있다. 경량 월드 모델(lightweight world model)은 온보드에서 로컬 움직임을 지속적으로 예측하고, 더 큰 온프레미스 모델은 시설 전체의 동역학(dynamics)을 유지하며, 클라우드 모델은 여러 사이트로부터 일반화된 표현(generalized representation)을 학습할 수 있다. 마찬가지로 로봇은 로컬 정책(local policy)을 실행하면서 온프레미스 최적화가 플릿 목표를 조정하고 클라우드 학습이 장기적으로 기본 정책을 개선하도록 구성할 수 있다.

각 계층이 물리적·경제적으로 가장 적합한 작업을 수행하면 자원 효율성(resource efficiency)이 향상된다. 로봇은 과도한 컴퓨팅 하드웨어를 탑재하지 않아도 되고, 온프레미스 인프라는 고가의 가속기를 로컬 플릿 전체에서 공유하며, 클라우드 플랫폼은 일시적인 대규모 작업을 위한 탄력적 자원(elastic resource)을 제공한다. 이를 통해 불필요한 하드웨어 중복을 줄이면서 점점 복잡해지는 물리 인공지능 모델에 필요한 계산 능력을 확보할 수 있다.

전력 및 열 제약(power and thermal constraint)은 이러한 분산 구조를 더욱 강화한다. 모바일 로봇은 제한된 에너지 공급과 냉각 능력으로 동작하기 때문에 모든 대규모 인공지능 모델을 지속적으로 실행하는 것은 현실적이지 않다. 고정형 온프레미스 서버는 더 높은 지속 전력을 지원할 수 있고, 클라우드 데이터센터는 더욱 큰 가속기 클러스터를 운용할 수 있다. 따라서 작업 배치는 단순한 정보기술 결정이 아니라 로봇의 에너지 및 열 설계(robot energy and thermal design)의 일부가 된다.

개인정보 보호(privacy), 보안(security), 데이터 주권(data sovereignty)은 계산 관점에서 최적인 배치를 변경할 수도 있다. 민감한 인간 데이터, 시설 지도, 산업 공정, 독점 정보는 로봇이나 로컬 인프라 내부에 유지해야 할 수 있다. 클라우드에는 익명화(anonymization), 요약 또는 명시적인 승인을 거친 정보만 전달할 수 있다. 따라서 계층 구조는 기술적 성능뿐 아니라 조직적 및 규제적 경계(organizational and regulatory boundary)도 고려해야 한다.

세 계층은 명확하게 정의된 장애 관계(failure relationship)를 가져야 한다. 클라우드의 장애가 로컬 플릿 운용을 중단시켜서는 안 되며, 온프레미스 장애가 로봇의 필수적인 자율성을 제거해서도 안 된다. 엣지 장애는 기계를 안전 상태(safe state)로 전환할 수 있는 저수준 안전 및 제어 메커니즘에 의해 격리되어야 한다. 이러한 의존성 구조는 상위 수준의 지능 서비스가 물리적 안전을 위협하는 단일 장애점(single point of failure)이 되는 것을 방지한다.

따라서 실용적인 역할 정의 과정은 각 작업의 물리적 결과와 시간 요구사항을 먼저 분석하고, 이후 계산 요구량, 데이터 양, 연결성 허용도, 에너지 비용, 개인정보 보호, 공유 요구사항을 평가하는 방식으로 이루어진다. 그런 다음 시간 또는 안전 제약을 위반하지 않으면서 해당 작업의 계산 요구사항을 충족할 수 있는 가장 낮은 계층에 기능을 배치할 수 있다. 상위 계층은 불필요한 의존성을 생성하는 것이 아니라 추가적인 기능과 지능을 제공해야 한다.

결과적으로 각 계층에는 명확하게 구분되는 책임이 부여된다. 엣지 컴퓨팅은 즉각적인 자율 지능(immediate autonomous intelligence)을 제공하고, 온프레미스 인프라는 공유 로컬 지능(shared local intelligence)을 제공하며, 클라우드 인프라는 글로벌 학습과 계산 확장성(computational scale)을 제공한다. 계층 사이의 경계는 유연하지만 우선순위는 서로 다르다. 엣지는 반응성과 독립성을, 온프레미스는 협업과 공유 자원을, 클라우드는 규모, 통합(aggregation), 장기적인 개선을 우선한다.

따라서 물리 인공지능은 엣지, 온프레미스, 클라우드 컴퓨팅 중 하나를 선택하는 것이 아니라 세 계층을 일관된 계산 계층 구조(coherent computational hierarchy)로 통합함으로써 이점을 얻는다. 센서 데이터는 로컬 세계 이해(local understanding)로 변환되고, 로컬 경험은 플릿 지식(fleet knowledge)이 되며, 플릿 지식은 글로벌 학습으로 발전한다. 그리고 개선된 지능은 다시 물리적 기계로 전달된다. 이러한 지속적인 루프를 통해 로봇은 로컬에서 반응성과 안전성을 유지하면서 점점 더 강력해지는 공유 지능 시스템(shared intelligence system)에 참여할 수 있다.

## 05.06. Real Time Inference at the Edge

![](images/image6.png){width="7.268055555555556in" height="7.268055555555556in"}

엣지에서의 실시간 추론(real-time inference at the edge)은 센서 관측(sensor observation)을 물리적 상호작용(physical interaction)이 요구하는 시간 제약 내에서 실행 가능한 지능(actionable intelligence)으로 변환하는 능력이다. 물리 인공지능(Physical AI)에서 추론은 단순히 계산 자원을 사용할 수 있을 때 완료하면 되는 요청--응답 연산(request-response computation)이 아니다. 그 출력은 물리적 상황이 변화하기 전에 로봇이 장애물을 인식하고, 움직임을 예측하며, 궤적을 수정하거나, 보호 동작(protective action)을 시작할 수 있는지를 결정할 수 있다.

실시간(real time)의 의미는 수행되는 기능에 따라 달라진다. 모터 제어(motor control)는 수백 또는 수천 헤르츠(hertz)의 실행 주기를 요구할 수 있지만, 시각 인식(visual perception)은 초당 수십 프레임으로 동작할 수 있고 상위 수준 추론(high-level reasoning)은 훨씬 낮은 주기로 실행될 수 있다. 따라서 엣지 추론(edge inference)은 하나의 보편적인 지연시간 목표가 아니라 작업별 마감시간(task-specific deadline)을 중심으로 설계되어야 한다. 유효한 시간 이후에 전달된 결과는 계산적으로 정확하더라도 운용 측면에서는 쓸모가 없을 수 있다.

종단간 지연시간(end-to-end latency)은 인공지능 모델이 실행되기 전부터 시작된다. 센서 노출(sensor exposure), 샘플링(sampling), 데이터 전송, 디코딩(decoding), 동기화(synchronization), 전처리(preprocessing), 메모리 이동(memory movement), 추론(inference), 후처리(postprocessing), 계획(planning), 명령 전송 모두 시간이 필요하다. 따라서 다른 단계가 파이프라인의 지연시간을 지배한다면 신경망 실행만 최적화해도 개선 효과가 제한될 수 있다. 실시간 물리 인공지능은 전체 센싱--의사결정 경로(sensing-to-decision path)를 하나의 통합된 지연시간 예산(latency budget)으로 다루어야 한다.

결정성(determinism)은 평균 추론 속도만큼 중요하다. 일반적으로 20밀리초에 완료되지만 때때로 150밀리초가 필요한 인식 모델은 하위 계획 시스템을 불안정하게 만들거나 오래된 환경 정보가 의사결정에 영향을 미치게 할 수 있다. 따라서 엣지 시스템은 평균 초당 프레임(frames per second)만 고려해서는 안 되며 최악 조건 지연시간(worst-case latency), 꼬리 지연시간(tail latency), 지터(jitter), 스케줄링 간섭(scheduling interference), 메모리 경합(memory contention), 일시적인 계산 과부하(compute overload)를 함께 고려해야 한다.

센서 파이프라인(sensor pipeline)은 실시간 추론에 상당한 부담을 발생시킨다. 여러 카메라, 라이다(LiDAR), 레이더(radar), 깊이 센서(depth sensor), 고유수용성 장치(proprioceptive device)가 서로 다른 갱신 주기와 데이터 양으로 동시에 동작할 수 있다. 이러한 관측은 융합(fusion) 전에 타임스탬프(timestamp)를 부여하고 동기화해야 하는 경우가 많다. 동기화나 전처리가 과도한 지연을 발생시키면 강력한 인공지능 가속기(AI accelerator)를 사용하더라도 시간적으로 일관되지 않은 정보가 입력되어 결과적인 월드 표현(world representation)의 품질이 저하될 수 있다.

이기종 컴퓨팅(heterogeneous computing)은 계산 특성에 따라 이러한 작업을 분산하는 데 도움이 된다. CPU는 오케스트레이션(orchestration), 통신, 순차 처리(sequential processing), 운영체제 서비스를 담당하고, GPU는 병렬 인식(parallel perception), 멀티모달 융합(multimodal fusion), 월드 모델 연산을 가속할 수 있다. NPU와 전용 가속기(specialized accelerator)는 최적화된 신경망을 효율적으로 실행하고, 마이크로컨트롤러(microcontroller)는 결정론적인 저수준 제어를 유지한다. 효과적인 실시간 추론은 하나의 장치 성능을 극대화하는 것이 아니라 이러한 프로세서를 적절하게 조정하는 데 달려 있다.

메모리 아키텍처(memory architecture)는 숨겨진 성능 제한 요소가 될 수 있다. 고해상도 카메라나 멀티모달 모델이 생성하는 대형 텐서(tensor)는 센서 인터페이스, CPU 메모리, GPU 메모리, 응용 프로세스 사이를 반복적으로 이동할 수 있다. 데이터 복사(copy), 형식 변환(format conversion), 동기화 장벽(synchronization barrier)은 지연시간 예산의 상당 부분을 소비할 수 있다. 따라서 제로 카피 파이프라인(zero-copy pipeline), 효율적인 메모리 할당(memory allocation), 하드웨어 가속 전처리, 적절하게 설계된 인터커넥트(interconnect)는 인공지능 모델 자체를 변경하지 않고도 실시간 성능을 향상시킬 수 있다.

모델 아키텍처(model architecture) 역시 배포 제약(deployment constraint)을 반영해야 한다. 벤치마크 정확도(benchmark accuracy)가 뛰어난 모델이라도 지연시간, 메모리 사용량(memory footprint), 전력 소비 때문에 온보드에서 예측 가능한 실행이 불가능하다면 적합하지 않을 수 있다. 소형 백본(smaller backbone), 효율적인 어텐션 메커니즘(efficient attention mechanism), 감소된 입력 해상도, 희소 연산(sparse computation), 조기 종료 아키텍처(early-exit architecture), 하드웨어 인지 연산자(hardware-aware operator)는 이론적인 모델 용량 일부를 실제 세계에서 더 유용한 반응성과 교환할 수 있다.

양자화(quantization), 가지치기(pruning), 지식 증류(knowledge distillation)는 모델을 엣지 하드웨어에 맞추기 위한 추가적인 방법을 제공한다. 저정밀 연산(lower-precision arithmetic)은 메모리 트래픽을 줄이고 지원되는 연산을 가속할 수 있으며, 가지치기는 불필요한 계산을 제거하고, 지식 증류는 대규모 모델의 행동을 더 작은 배포 가능 네트워크에 전달할 수 있다. 이러한 최적화 기법은 로봇 운용 환경에 필요한 정확도와 강건성(robustness)을 유지할 수 있을 때 특히 유용하다.

모든 인식 또는 추론 작업을 동일한 주기로 실행할 필요는 없다. 빠른 장애물 탐지(obstacle detection)는 지속적으로 실행할 수 있지만, 의미론적 장면 해석(semantic scene interpretation)은 더 낮은 주기로 갱신하고, 계산 비용이 높은 추론은 비정상적인 조건이 발생할 때만 활성화할 수 있다. 다중 주기 추론(multi-rate inference)은 각 기능의 시간적 중요도에 따라 계산 자원을 배분한다. 이를 통해 즉각적인 물리적 안전에 직접 영향을 주는 기능은 높은 갱신 주기를 유지하면서 불필요한 계산을 줄일 수 있다.

이벤트 기반 추론(event-driven inference)은 이러한 원리를 더욱 확장할 수 있다. 관측이 예측 가능한 상태를 유지하고 환경 변화가 작을 때 일부 고비용 모듈은 낮은 주기로 실행하거나 비활성 상태로 유지할 수 있다. 반대로 큰 움직임, 불확실성(uncertainty), 새로움(novelty), 작업 전환(task transition), 안전 관련 이벤트가 발생하면 더 높은 계산량을 활성화할 수 있다. 이러한 적응형 계산(adaptive computation)은 물리 인공지능 시스템이 항상 최대 추론 부하를 유지하는 대신 환경 복잡도(environmental complexity)에 따라 처리 능력을 할당하도록 한다.

동적 추론(dynamic inference)은 모델 충실도(model fidelity) 자체를 변경할 수도 있다. 단순한 환경에서 동작하는 로봇은 낮은 해상도의 입력, 짧은 예측 구간(prediction horizon), 적은 모델 계층, 단순한 정책(policy)을 사용할 수 있다. 불확실성이나 환경 복잡도가 증가하면 더 높은 비용의 처리를 활성화할 수 있다. 이를 통해 상황의 난이도와 계산 자원 할당(compute allocation)을 연결하여 엣지 지능이 반응성, 정확도, 에너지 소비, 열 제약(thermal constraint)의 균형을 조절할 수 있다.

여러 인공지능 작업이 동일한 가속기를 공유하면 스케줄링(scheduling)이 중요해진다. 인식, 위치 추정, 월드 모델링, 예측, 계획, 언어 조건부 추론(language-conditioned reasoning)이 GPU 시간과 메모리를 두고 경쟁할 수 있다. 안전과 관련된 작업은 선택적인 분석이나 의미론적 정보 확장보다 높은 우선순위를 가져야 한다. 마감시간 인지 스케줄링(deadline-aware scheduling), 자원 예약(resource reservation), 작업 격리(workload isolation), 제한된 실행 시간(bounded execution)은 계산 비용이 높은 보조 작업이 중요한 인식을 지연시키는 것을 방지할 수 있다.

전력 제한(power limitation)은 지속적인 추론 성능을 추가로 제약한다. 가속기를 최대 성능으로 실행하면 지연시간을 줄일 수 있지만 배터리 소비와 발열을 증가시킨다. 따라서 최적 운용점(optimal operating point)은 반드시 하드웨어의 최대 주파수와 일치하지 않는다. 동적 전압 및 주파수 조정(dynamic voltage and frequency scaling), 작업 인지 전력 모드(workload-aware power mode), 선택적 모델 활성화, 가속기 활용률 관리를 통해 추론 성능과 로봇 운용 시간 사이의 균형을 조절할 수 있다.

열 거동(thermal behavior)은 보다 장기적인 타이밍 문제를 발생시킨다. 엣지 플랫폼이 처음에는 지연시간 목표를 충족하더라도 시간이 지나면서 온도가 상승하여 열 스로틀링(thermal throttling)이 발생하면 가속기 주파수가 감소할 수 있다. 따라서 짧은 벤치마크만으로 검증된 시스템은 장시간 임무에서 요구 성능을 만족하지 못할 수 있다. 실시간 성능 검증은 실제 주변 온도, 인클로저(enclosure), 센서 구성, 작업 부하 조건에서 장시간 운용하여 열 평형(thermal equilibrium)에 도달한 이후에도 추론 마감시간을 충족하는지 확인해야 한다.

실시간 추론은 불확실성 추정(uncertainty estimation)과 통합되어야 한다. 모델이 관측 내용을 충분히 신뢰하지 못한다면 빠른 출력만으로는 충분하지 않다. 인식 및 예측 시스템은 신뢰도(confidence), 불일치(disagreement), 새로움, 불확실성 지표를 하위 의사결정 로직에 제공할 수 있다. 불확실성이 운용상 중요한 수준으로 증가하면 로봇은 속도를 낮추고, 안전 여유(safety margin)를 확대하며, 추가 센서를 활성화하거나, 더 높은 수준의 추론을 요청하거나, 보수적인 행동(conservative behavior)으로 전환할 수 있다.

엣지 컴퓨터가 과부하될 경우 점진적 성능 저하(graceful degradation)가 필수적이다. 마감시간이 예측 불가능하게 실패하도록 두는 대신 카메라 해상도를 낮추고, 중요하지 않은 프레임을 건너뛰며, 예측 구간을 단축하고, 선택적 모델을 비활성화하거나, 더 단순한 폴백 네트워크(fallback network)로 전환할 수 있다. 필수적인 인식, 위치 추정, 안전 모니터링, 제어 인터페이스는 보호되어야 한다. 따라서 계산 성능의 저하는 운용 중요도(operational criticality)에 따라 의도적이고 순차적으로 이루어져야 한다.

외부 인프라는 실시간 엣지 추론을 보완할 수 있지만 안전 중요 타이밍 루프(safety-critical timing loop) 내부에 위치해서는 안 된다. 온프레미스 서버(on-premise server)는 더 높은 수준의 의미론적 추론, 대규모 월드 모델, 플릿 수준 정보를 제공할 수 있으며, 클라우드는 더욱 큰 모델과 글로벌 지식(global knowledge)을 제공할 수 있다. 로봇은 이러한 출력을 사용할 수 있을 때 활용할 수 있지만 네트워크 지연시간이 증가하거나 연결이 끊어져도 즉각적인 의사결정은 계속 가능해야 한다.

실시간 추론은 시간적 재사용(temporal reuse)을 통해서도 효율성을 향상시킬 수 있다. 연속된 센서 관측은 서로 강하게 연관되어 있는 경우가 많으므로 모든 표현을 매번 처음부터 완전히 다시 계산하는 것은 비효율적일 수 있다. 특징 캐싱(feature caching), 시간 메모리(temporal memory), 추적(tracking), 순환 상태(recurrent state), 증분 매핑(incremental mapping), 잠재 상태 전파(latent-state propagation)를 통해 이전 계산 결과를 재사용할 수 있다. 이러한 접근 방식은 동적으로 변화하는 환경에 대한 로봇의 이해 연속성을 유지하면서 계산 요구량을 줄일 수 있다.

따라서 프로파일링(profiling)은 최종적인 최적화 단계가 아니라 기본적인 엔지니어링 활동이다. 개발자는 실제 작업 부하에서 센서, 전처리, 추론 엔진(inference engine), 메모리 전송, 후처리, 하위 소비 모듈(downstream consumer)의 지연시간 분포를 측정해야 한다. GPU 활용률(utilization), 메모리 대역폭(memory bandwidth), 큐 깊이(queue depth), 온도, 전력, 프레임 손실(frame drop), 마감시간 실패(deadline miss)는 모델 추론 시간만 측정하는 것보다 시스템의 실제 상태를 더 완전하게 보여준다.

궁극적으로 적절한 성능 지표(performance metric)는 단순한 추론 처리량이 아니라 물리적 효과성(physical effectiveness)이다. 초당 프레임, TOPS, 가속기 활용률은 유용한 엔지니어링 지표이지만 로봇이 행동 마감시간 이전에 충분히 정확한 정보를 얻었는지를 직접 측정하지는 않는다. 따라서 실시간 엣지 추론은 센싱--행동 지연시간(sensing-to-action latency), 마감시간 충족률(deadline satisfaction), 부하 상황에서의 강건성, 에너지 효율성(energy efficiency), 그리고 최종적으로 나타나는 자율 행동(autonomous behavior)을 기준으로 평가해야 한다.

따라서 엣지에서의 실시간 추론은 하드웨어--소프트웨어--인공지능 공동 설계(hardware--software--AI co-design) 문제이다. 센서 구성(sensor configuration)은 데이터 부하를 결정하고, 모델 아키텍처는 계산 요구량을 결정하며, 가속기는 사용 가능한 처리량을 결정하고, 메모리 시스템은 데이터 이동 비용을 결정한다. 전력과 냉각은 지속 가능한 성능을 결정하며, 스케줄링과 폴백 정책은 실제 조건이 정상적인 가정에서 벗어났을 때 이러한 자원이 어떻게 동작할지를 결정한다.

이러한 요소들이 함께 설계될 때 엣지 추론은 인공지능과 물리적 행동 사이의 연결 고리(bridge)가 된다. 센서는 변화하는 세계를 지속적으로 관측하고, 최적화된 모델은 관측을 유용한 상태와 예측으로 변환하며, 플래너(planner)는 이러한 표현을 의사결정으로 변환하고, 결정론적 제어(deterministic control)는 액추에이터를 통해 이를 실행한다. 이러한 루프를 제한되고 예측 가능한 시간 안에서 유지하는 것이 물리 인공지능이 현실 세계와 안전하고, 신속하며, 자율적으로 상호작용할 수 있도록 하는 핵심 조건이다.

## 05.07. Heavy Reasoning and Training Off Robot

![](images/image7.png){width="7.268055555555556in" height="7.268055555555556in"}

고부하 추론 및 학습(heavy reasoning and training)은 일반적으로 로봇 외부(off-robot)에서 실행되어야 한다. 이러한 작업의 계산 특성이 즉각적인 물리적 상호작용에 필요한 작업과 근본적으로 다르기 때문이다. 로봇은 엄격한 전력 및 열 제약(power and thermal limits) 안에서 저지연 인식(low-latency perception), 계획(planning), 제어(control)를 유지해야 하지만, 대규모 추론과 학습은 방대한 메모리, 다수의 가속기(accelerator), 긴 실행 시간, 대규모 데이터셋을 요구할 수 있다. 이러한 작업을 분리하면 각각의 환경을 실제 역할에 맞게 최적화할 수 있다.

고부하 추론(heavy reasoning)은 지속적인 온보드 추론(onboard inference)의 현실적인 범위를 넘어서는 계산 과정을 포함한다. 대규모 멀티모달 모델(multimodal model), 파운데이션 모델(foundation model), 장기 계획기(long-horizon planner), 복잡한 월드 모델(world model), 대규모 탐색(search), 다단계 추론 시스템(multi-stage reasoning system)은 밀리초가 아니라 수 초 또는 수 분의 처리 시간을 필요로 할 수 있다. 이러한 시스템의 목적은 높은 주기로 액추에이터를 직접 제어하는 것보다 복잡한 상황을 해석하고, 작업을 분해하며, 대안을 평가하고, 전략적 지침을 제공하는 데 있다.

운용 지연시간(operational latency)이 여전히 중요하다면 이러한 작업은 가까운 온프레미스 인프라(on-premise infrastructure)에 배치할 수 있다. 로컬 GPU 서버 또는 가속기 클러스터(accelerator cluster)는 모바일 로봇보다 더 큰 모델을 실행하면서 상대적으로 낮은 지연시간의 시설 네트워크를 통해 연결될 수 있다. 이를 통해 로봇은 이러한 기능을 지속적으로 실행하는 데 필요한 모든 계산 하드웨어를 탑재하지 않고도 심층 의미론적 해석(deep semantic interpretation), 어려운 장면 분석, 장기 계획, 공유 월드 모델 추론(shared world-model reasoning)을 요청할 수 있다.

클라우드 인프라(cloud infrastructure)는 작업이 더욱 큰 규모의 계산 자원을 요구하거나 더 긴 통신 지연을 허용할 수 있을 때 이러한 접근법을 확장한다. 초대형 파운데이션 모델, 전역 최적화(global optimization), 광범위한 탐색, 사이트 간 추론(cross-site reasoning), 대규모 시뮬레이션 캠페인(simulation campaign), 플릿 전체 분석(fleet-wide analytics)은 탄력적인 컴퓨팅 자원(elastic computing resource)을 활용할 수 있다. 이러한 작업은 안전 중요 제어 루프(safety-critical control loop) 내부에 배치되지 않기 때문에 로봇은 원격 시스템으로부터 추가 정보나 권고를 기다리는 동안에도 로컬에서 계속 동작할 수 있다.

학습(training)은 로봇 외부 계산을 사용해야 하는 더욱 강력한 이유를 제공한다. 현대의 물리 인공지능 모델(Physical AI model)은 수백만 또는 수십억 개의 학습 예제, 반복적인 순전파 및 역전파(forward and backward pass), 옵티마이저 상태(optimizer state), 대규모 활성화 메모리(activation memory), 분산 통신(distributed communication), 방대한 체크포인트 저장공간(checkpoint storage)을 요구할 수 있다. 이러한 요구사항은 추론과 근본적으로 다르다. 배터리 기반 로봇에서 전체 규모의 학습을 지속적으로 수행하면 과도한 에너지를 소비하고 상당한 열을 발생시키며 자율 운용에 필요한 자원과 경쟁하게 된다.

데이터가 시설 내부에 유지되어야 하거나 실제 운용과 가까운 위치에서 빠른 반복 개발이 필요한 경우 온프레미스 학습(on-premise training)이 유용할 수 있다. 선택된 로봇 경험(robot experience)을 로컬 저장소로 전송하고 데이터셋으로 큐레이션(curation)하여 미세조정(fine-tuning), 적응(adaptation), 보정(calibration), 평가(evaluation)에 사용할 수 있다. 로컬 가속기 서버는 민감한 원시 정보를 조직 외부로 전송하지 않고 특화된 모델을 업데이트할 수 있어 계산 능력과 데이터 지역성(data locality) 사이의 실용적인 절충점을 제공한다.

데이터셋과 모델이 로컬 인프라의 용량을 초과하면 클라우드 학습(cloud training)이 유용해진다. 여러 로봇과 사이트에서 수집된 경험을 더 큰 학습 코퍼스(training corpus)로 통합할 수 있으며, 분산 GPU 클러스터(distributed GPU cluster)를 통해 인식 네트워크, 월드 모델, 멀티모달 시스템, 정책(policy), 파운데이션 모델을 학습할 수 있다. 탄력적 자원을 활용하면 모든 로봇 배포 위치에 동일한 하드웨어를 영구적으로 설치하지 않고도 일시적으로 대규모 계산 능력을 확보할 수 있다.

추론과 학습의 분리가 로봇을 수동적인 데이터 수집기(passive data collector)로 만든다는 의미는 아니다. 엣지 시스템(edge system)은 어떤 경험이 학습에 가치가 있는지를 능동적으로 판단할 수 있다. 낮은 신뢰도의 예측, 예상하지 못한 장애물, 인간 개입(human intervention), 계획 실패(planning failure), 비정상적인 센서 조합, 새로운 객체, 중요한 환경 변화 등을 온보드에서 식별할 수 있다. 이러한 선택적 수집(selective collection)은 불필요한 데이터 이동을 줄이면서 학습 자원을 정보 가치가 높은 사례에 집중시킨다.

온프레미스 인프라는 이러한 정보가 대규모 학습 시스템으로 전달되기 전에 추가적으로 정제할 수 있다. 중복 관측을 제거하고, 이벤트를 색인화(indexing)하며, 센서 스트림을 동기화하고, 메타데이터(metadata)를 추가하며, 개인정보에 민감한 내용을 필터링할 수 있다. 어려운 시나리오는 재생(replay)하고 여러 로봇 사이에서 비교할 수 있다. 이렇게 큐레이션된 데이터셋은 모든 센서 프레임을 무차별적으로 업로드하는 것보다 유용하며 저장, 네트워크, 학습 비용을 크게 줄일 수 있다.

따라서 지속적 학습 루프(continuous learning loop)는 모든 계산 계층에 걸쳐 형성될 수 있다. 로봇은 물리적 경험을 생성하고, 엣지 지능(edge intelligence)은 의미 있는 이벤트를 선택하며, 온프레미스 시스템은 로컬 지식을 통합하고 큐레이션하며, 클라우드 인프라는 대규모 학습을 수행한다. 개선된 모델은 검증 및 배포 단계를 거쳐 계층 구조 아래로 다시 이동하고 최적화된 버전이 로봇으로 돌아간다. 이를 통해 물리적 상호작용 자체가 미래 행동을 지속적으로 개선하기 위한 데이터 원천이 된다.

고부하 추론 역시 로봇을 직접 제어하지 않으면서 동일한 루프에 참여할 수 있다. 대규모 로봇 외부 모델(off-robot model)은 어려운 에피소드(episode)를 분석하고, 로컬 정책이 실패한 이유를 설명하며, 대안적인 전략을 생성하거나, 부족한 지식을 식별할 수 있다. 이러한 결과는 데이터셋 생성, 시뮬레이션, 정책 개선, 엔지니어링 분석을 지원할 수 있다. 따라서 고부하 추론의 가치는 즉각적인 답변 제공을 넘어 자율 시스템의 체계적인 개선에도 기여할 수 있다.

시뮬레이션(simulation)은 로봇 외부 학습과 밀접하게 연결되어 있다. 대규모 컴퓨팅 클러스터는 많은 가상 환경을 병렬로 실행하여 실제 환경에서 재현하기에 비용이 높거나 느리거나 위험한 다양한 조건에 정책과 모델을 노출할 수 있다. 지형, 조명, 객체 배치, 센서 노이즈(sensor noise), 액추에이터 특성, 인간 행동, 통신 장애, 희귀 위험 사건(rare hazardous event)을 체계적으로 탐색한 후 개선된 지능을 실제 기계에 배포할 수 있다.

시뮬레이션을 통해 생성된 합성 데이터(synthetic data)는 실제 로봇 데이터를 보완할 수 있다. 실제 관측은 환경적 현실성(environmental authenticity)을 제공하고, 시뮬레이션 환경은 희귀하거나 충분히 표현되지 않은 조건의 범위를 의도적으로 확대할 수 있다. 로봇 외부 인프라는 학습과 평가 과정에서 이러한 데이터 원천을 결합할 수 있다. 목적은 실제 경험을 대체하는 것이 아니라 물리 인공지능 시스템이 학습할 수 있는 상황의 범위를 확장하는 것이다.

대규모 월드 모델(large world model)은 물리 환경의 시간적 표현(temporal representation)을 학습하는 과정에서 긴 시퀀스(sequence), 대규모 메모리, 높은 가속기 용량이 필요할 수 있으므로 로봇 외부 자원의 이점을 특히 크게 얻는다. 모델은 객체의 움직임, 행동에 따른 상태 변화, 에이전트(agent) 간 상호작용, 현재 조건에 따른 미래 관측 변화를 학습할 수 있다. 학습 후에는 더 작거나 압축된 버전을 엣지에 배포하여 단기 예측을 수행하고, 대형 버전은 심층 추론을 위해 로봇 외부에 유지할 수 있다.

지식 증류(knowledge distillation)는 고성능 로봇 외부 지능과 경량 엣지 지능(lightweight edge intelligence)을 연결하는 중요한 방법을 제공한다. 대규모 교사 모델(teacher model)은 방대한 계산 자원과 데이터셋을 이용해 학습하고, 더 작은 학생 모델(student model)은 온보드 제약조건 안에서 가장 유용한 행동을 재현하도록 학습할 수 있다. 양자화(quantization), 가지치기(pruning), 아키텍처 최적화(architecture optimization), 하드웨어 특화 컴파일(hardware-specific compilation)을 통해 고비용 학습으로 획득한 능력을 유지하면서 배포 모델을 더욱 축소할 수 있다.

이는 비대칭적인 계산 흐름(asymmetric flow of computation)을 만든다. 학습 과정에서는 로봇 외부에서 막대한 계산 자원을 투입하지만, 그 결과 비교적 효율적인 추론을 온보드에서 수행할 수 있게 된다. 하나의 모델을 학습하는 데 수천 가속기 시간(accelerator-hour)이 필요하더라도 최적화 이후에는 수 밀리초 안에 실행할 수 있다. 물리 인공지능 아키텍처는 로봇이 모델 개발에 사용된 계산 환경 자체를 재현하도록 하는 대신 이러한 비대칭성을 적극적으로 활용해야 한다.

고부하 추론은 지속적으로 실행하는 대신 선택적으로 호출할 수도 있다. 대부분의 일상적인 상황은 빠른 온보드 모델로 처리하고, 모호하거나 새로운 상황, 전략적으로 복잡한 상황, 낮은 신뢰도의 상황이 발생할 때 더 큰 온프레미스 모델로 에스컬레이션(escalation)할 수 있다. 더 많은 지식이나 계산이 필요한 문제는 클라우드 서비스로 이동할 수 있다. 이를 통해 상황이 요구하는 경우에만 계산 노력을 증가시키는 계층형 추론 프로세스(hierarchical reasoning process)를 구성할 수 있다.

그러나 로봇은 로봇 외부 추론 결과를 기다리는 동안에도 안전하게 행동할 수 있어야 한다. 기존에 알려진 안전 행동을 계속하거나, 속도를 낮추거나, 정지하거나, 현재 위치를 유지하거나, 안전 여유(safety margin)를 증가시키거나, 보수적인 폴백 정책(conservative fallback policy)을 실행할 수 있다. 서버를 사용할 수 없다는 이유로 기본적인 자율성이 사라져서는 안 되며 선택적 지능만 일시적으로 제한되어야 한다. 따라서 고부하 추론은 향상된 기능을 제공하고 엣지 지능은 운용 연속성(operational continuity)을 유지한다.

네트워크 설계(network design)는 이러한 아키텍처가 얼마나 효과적으로 동작하는지에 영향을 미친다. 대규모 센서 기록이나 모델 상태(model state)를 업로드하면 상당한 대역폭을 소비할 수 있으며, 대규모 모델을 다시 로봇으로 전달할 때도 신중한 배포 스케줄링이 필요하다. 압축(compression), 캐싱(caching), 증분 업데이트(incremental update), 우선순위 기반 전송(prioritized transfer), 로컬 모델 저장소(local model repository)를 통해 통신 오버헤드(communication overhead)를 줄일 수 있다. 온프레미스 인프라는 로봇과 지리적으로 먼 클라우드 자원 사이의 중간 캐시 및 배포 지점으로 활용할 수 있다.

보안(security)과 개인정보 보호(privacy) 역시 무제한적인 중앙집중화보다 통제된 로봇 외부 처리를 선호하게 만드는 요인이다. 민감한 원시 관측은 로봇이나 시설 내부에 유지하고, 파생된 특징(derived feature), 익명화된 샘플(anonymized sample), 통계 또는 승인된 데이터셋만 상위 계층으로 전달할 수 있다. 학습 시스템은 데이터 출처 추적성(provenance), 접근 제어(access control), 모델 무결성(model integrity), 추적 가능성(traceability)을 유지하여 로봇으로 다시 전달되는 지식이 물리적 행동에 영향을 미치기 전에 검증될 수 있도록 해야 한다.

새롭게 학습된 지능이 실제 운용 로봇으로 돌아가기 전에 모델 검증(model validation)이 반드시 이루어져야 한다. 후보 모델은 오프라인 데이터셋, 시뮬레이션 시나리오, 회귀 테스트(regression test), 안전 사례(safety case), 기록된 실패 에피소드를 이용해 평가할 수 있다. 이후 온프레미스 인프라에서 사이트별 검증(site-specific validation)을 수행한 뒤 단계적으로 배포할 수 있다. 허용 가능한 행동이 입증된 이후에만 새로운 모델이 기존 엣지 모델을 대체하거나 보완해야 한다.

로봇 외부 인프라는 여러 모델 세대(model generation)에 걸친 지속적인 평가도 가능하게 한다. 새로운 모델을 일관된 벤치마크와 재생된 운용 시나리오를 사용하여 이전 버전과 비교할 수 있다. 평균 정확도의 향상뿐 아니라 강건성(robustness), 지연시간, 불확실성, 에너지 요구량, 안전 관련 실패 모드(safety-relevant failure mode)를 함께 평가해야 한다. 더 크거나 더 강력한 모델이라도 배포 특성 때문에 전체 로봇 시스템의 신뢰성이 저하된다면 반드시 더 좋은 모델이라고 할 수는 없다.

온보드와 로봇 외부 계산 사이의 적절한 경계는 가속기, 네트워크, 모델이 발전함에 따라 계속 변화할 것이다. 현재 서버가 필요한 기능도 향후 모바일 하드웨어에서 효율적으로 실행될 수 있으며, 반대로 더욱 정교한 추론 시스템은 계속 확장되어 엣지 용량을 넘어설 수 있다. 따라서 아키텍처는 작업 배치(workload placement)를 고정된 것으로 간주하지 않고 모델과 기능이 엣지, 온프레미스, 클라우드 계층 사이에서 이동할 수 있도록 적응 가능하게 설계되어야 한다.

궁극적으로 로봇 외부의 고부하 추론 및 학습(heavy reasoning and training off-robot)은 물리 인공지능이 서로 상충할 수 있는 두 가지 요구사항, 즉 즉각적인 자율 행동(immediate autonomous behavior)과 계산 집약적인 지능 개발(computationally intensive intelligence development)을 동시에 충족할 수 있도록 한다. 로봇은 효율적인 로컬 추론을 통해 높은 반응성을 유지하고, 대규모 인프라는 심층 추론, 시뮬레이션, 학습, 플릿 학습(fleet learning), 모델 개선을 수행한다. 이러한 계층 구조를 통해 물리적 기계는 엄격한 실시간 제약 안에서 동작하면서도 자체적으로 탑재할 수 있는 수준을 훨씬 넘어서는 계산 자원의 이점을 활용할 수 있다.

## 05.08. Model Placement and Workload Partitioning

![](images/image8.png){width="7.268055555555556in" height="7.268055555555556in"}

모델 배치(model placement)와 작업 부하 분할(workload partitioning)은 각각의 물리 인공지능(Physical AI) 기능을 로봇 엣지(robot edge), 온프레미스 인프라(on-premise infrastructure), 클라우드 자원(cloud resource) 중 어디에서 실행해야 하는지를 결정한다. 목표는 단순히 계산 비용이 높은 작업을 로봇 외부로 이동시키는 것이 아니라, 각 작업을 지연시간(latency), 안전성(safety), 연산 능력(compute), 메모리(memory), 에너지(energy), 연결성(connectivity), 데이터 요구사항을 가장 잘 충족하는 환경에 배치하는 것이다. 따라서 효과적인 배치는 계층형 물리 인공지능(hierarchical Physical AI)의 핵심 시스템 설계 문제가 된다.

유용한 출발 원칙은 운용 제약조건(operational constraint)을 위반하지 않으면서 작업을 안정적으로 실행할 수 있는 가장 낮은 계산 계층(computational layer)에 작업을 배치하는 것이다. 물리 세계와 즉각적으로 상호작용해야 하는 기능은 자연스럽게 온보드 실행(onboard execution)에 적합하고, 더 큰 모델이나 더 광범위한 정보를 요구하는 작업은 상위 계층으로 이동할 수 있다. 이러한 원칙은 불필요한 의존성을 최소화하면서 모바일 하드웨어의 한계 안에서 구현하기 어려운 기능을 상위 계층이 제공하도록 한다.

지연시간(latency)은 가장 강력한 배치 기준 중 하나이다. 충돌 회피(collision avoidance), 위치 추정(localization), 상태 추정(state estimation), 단기 예측(short-horizon prediction), 로컬 계획(local planning)은 밀리초 또는 수십 밀리초 이내의 응답을 요구하는 경우가 많다. 이러한 작업을 외부 네트워크를 통해 전송하면 가변적인 지연과 지터(jitter)가 발생할 수 있다. 따라서 지연된 출력이 안정성, 반응성 또는 안전성을 저하시킬 수 있는 기능을 지원하는 모델은 가능한 한 센서와 액추에이터 가까이에 유지해야 한다.

안전 중요도(safety criticality)는 이러한 로컬 배치를 더욱 강화한다. 모델 출력이 즉각적인 물리적 움직임에 직접적으로 영향을 줄수록 시스템은 원격 인프라에 덜 의존해야 한다. 보호 인식(protective perception), 비상 행동(emergency behavior), 로컬 궤적 검증(local trajectory validation), 필수 상태 추정은 통신이 실패하더라도 계속 동작해야 한다. 상위 수준 추론은 이러한 기능을 향상시킬 수 있지만 외부 서비스의 손실로 인해 로봇의 최소 안전 자율 행동(minimum safe autonomous behavior)이 제거되어서는 안 된다.

연산 요구량(compute demand)은 반대 방향의 압력을 만든다. 대규모 멀티모달 모델(multimodal model), 파운데이션 모델(foundation model), 장기 월드 모델(long-horizon world model), 복잡한 최적화(complex optimization), 시뮬레이션(simulation), 학습(training)은 온보드 가속기의 메모리, 처리량(throughput), 지속 가능한 전력 범위를 초과할 수 있다. 이러한 작업은 더 큰 GPU, 다수의 가속기, 더 많은 메모리 용량, 강력한 냉각 시스템을 사용할 수 있는 온프레미스 또는 클라우드 인프라로 이동시킬 수 있다.

그러나 모델 크기(model size)만으로 배치 위치를 결정해서는 안 된다. 비교적 큰 모델이라도 엄격한 마감시간 안에 출력이 반드시 필요하다면 압축(compression)이나 전용 하드웨어를 사용하여 엣지에서 실행해야 할 수 있다. 반대로 가끔만 사용되는 작은 모델은 통신 지연을 허용할 수 있다면 공유 인프라에서 실행하는 것이 더 효율적일 수 있다. 따라서 배치 결정에서는 매개변수 수(parameter count)나 계산 복잡도만이 아니라 모델의 전체적인 운용 역할(operational role)을 고려해야 한다.

온프레미스 인프라(on-premise infrastructure)는 중요한 중간 영역을 제공한다. 지속적인 온보드 실행에는 계산 비용이 너무 높지만 비교적 낮은 통신 지연시간을 요구하는 모델은 로컬 GPU 서버에서 동작할 수 있다. 고부하 의미론적 해석(heavy semantic interpretation), 대규모 월드 모델 추론, 공유 매핑(shared mapping), 플릿 최적화(fleet optimization), 어려운 장면 추론(difficult-scene reasoning)을 로컬에서 제공할 수 있다. 여러 로봇이 이러한 자원을 공유함으로써 완전한 클라우드 의존성을 만들지 않으면서 하드웨어 활용률을 향상시킬 수 있다.

즉각적인 응답보다 규모(scale)가 중요한 경우에는 클라우드 배치(cloud placement)가 적합하다. 대규모 학습, 글로벌 플릿 분석(global fleet analytics), 파운데이션 모델 개발, 광범위한 시뮬레이션, 사이트 간 학습(cross-site learning), 장기 최적화(long-term optimization), 대규모 데이터셋 처리는 훨씬 긴 실행 시간을 허용할 수 있다. 클라우드 자원은 이러한 작업에 맞추어 일시적으로 확장할 수 있으므로 모든 시설에 동일한 계산 인프라를 설치하지 않고도 매우 큰 계산 능력을 사용할 수 있다.

작업 부하 분할(workload partitioning)은 완전한 모델 단위로 수행할 수 있지만 하나의 지능 기능을 여러 계층으로 나누는 것도 가능하다. 로봇은 소형 인식 모델이나 월드 모델을 지속적으로 실행하고, 온프레미스 서버에서는 더 큰 버전을 이용해 심층적인 해석을 수행할 수 있다. 클라우드 모델은 여러 사이트에서 일반화된 표현(generalized representation)을 학습할 수 있다. 따라서 동일한 기능적 능력이 하나의 위치에만 존재하는 것이 아니라 여러 계산 규모에서 구현될 수 있다.

신경망(neural network) 자체도 경우에 따라 분할할 수 있다. 초기 계층(early layer)은 로봇에서 원시 센서 정보를 처리하여 중간 특징(intermediate feature)을 생성하고, 이후 계층은 외부 인프라에서 실행할 수 있다. 이러한 분할 컴퓨팅(split computing) 방식은 원시 데이터 전송량을 줄이면서 더 큰 후단 모델이 서버 자원을 활용하도록 할 수 있다. 실제 유용성은 특징 데이터 크기, 통신 대역폭, 개인정보 보호, 지연시간, 동기화, 각 분할 영역의 계산 비용에 따라 달라진다.

데이터 지역성(data locality)은 또 다른 주요 배치 요소이다. 원시 카메라 스트림, 라이다 포인트 클라우드(LiDAR point cloud), 오디오, 시설 지도, 인간 상호작용 데이터는 지속적으로 전송하기에는 지나치게 크거나 민감할 수 있다. 엣지 모델은 이러한 스트림을 압축된 특징, 객체, 이벤트, 임베딩(embedding), 요약 정보로 변환할 수 있다. 온프레미스 시스템은 이를 통합하고 큐레이션(curation)하며, 더 광범위한 학습 또는 조직적 가치가 있는 정보만 클라우드 인프라로 이동시키면 된다.

따라서 대역폭(bandwidth)은 하나의 계산 자원(computational resource)처럼 다루어야 한다. 모델을 외부로 오프로딩(offloading)하면 온보드 계산량은 감소하지만 네트워크 트래픽이 증가하며, 경우에 따라 통신 비용이 절감된 계산 비용보다 커질 수 있다. 특히 고해상도 센서 데이터를 지속적으로 전송하는 것은 비용이 높다. 배치 분석에서는 로컬 실행 비용과 전송 지연시간, 대역폭 소비, 네트워크 가용성(network availability), 원격에서 정보를 재구성하거나 처리하는 비용을 함께 비교해야 한다.

연결성 허용도(connectivity tolerance)는 또 하나의 명확한 분할 기준을 제공한다. 네트워크 장애 중에도 반드시 유지되어야 하는 기능은 엣지에 속한다. 시설 네트워크의 일시적인 중단을 허용할 수 있는 기능은 온프레미스에 배치할 수 있으며, 광역 네트워크 연결을 기다릴 수 있는 작업은 클라우드에서 실행할 수 있다. 이를 통해 상위 계산 계층을 일시적으로 사용할 수 없더라도 로봇이 필수적인 자율성을 유지하는 계층형 의존 구조(hierarchical dependency)를 형성할 수 있다.

작업 실행 빈도(workload frequency) 역시 배치에 영향을 미친다. 지속적으로 실행되는 모델은 온보드에 영구적으로 배치하고 최적화할 가치가 있지만, 비정상적인 이벤트에서만 호출되는 고비용 모델은 공유 인프라에서 실행하는 것이 더 효율적일 수 있다. 따라서 일상적인 상황은 경량 엣지 모델(lightweight edge model)로 처리하고, 불확실성, 새로움(novelty), 어려운 작업이 발생하면 더 큰 온프레미스 또는 클라우드 모델로 에스컬레이션(escalation)할 수 있다. 운용 상황이 요구할 때만 계산 집약도를 증가시키는 것이다.

이러한 에스컬레이션 메커니즘은 계층형 추론(hierarchical reasoning)을 가능하게 한다. 빠른 엣지 모델이 초기 의사결정을 수행하고 신뢰도(confidence)를 추정할 수 있다. 신뢰도가 충분하면 로봇은 로컬에서 계속 동작한다. 불확실성이 임계값(threshold)을 초과하면 더 큰 로컬 서버 모델이 상황을 분석할 수 있다. 더 많은 문맥(context)이나 계산을 요구하는 문제는 클라우드 서비스로 이동할 수 있다. 로봇은 상위 계층의 응답을 기다리는 동안에도 안전한 폴백 행동(safe fallback behavior)을 유지해야 한다.

작업은 시간 범위(time horizon)에 따라서도 분할할 수 있다. 즉각적인 상태 추정과 예측은 온보드에서 수행하고, 시설 수준의 계획 및 협업은 온프레미스에서 실행하며, 장기 학습(long-term learning) 또는 전역 최적화(global optimization)는 클라우드에서 수행할 수 있다. 이러한 시간적 분할(temporal partitioning)은 밀리초, 초, 분, 시간, 그리고 그 이상의 시간 범위에서 서로 다른 의사결정이 발생하는 물리 인공지능의 자연스러운 구조와 계산 배치를 일치시킨다.

메모리 요구사항(memory requirement)은 계산 처리량과 함께 고려되어야 한다. 대규모 모델은 가속기의 이론적인 계산 능력 안에 들어오더라도 활성화 값(activation), 시간적 문맥(temporal context), 캐시(cache), 동시에 실행되는 여러 모델을 포함하면 실제 장치 메모리를 초과할 수 있다. 온보드 시스템은 또한 인식, 매핑, 계획, 운영체제 프로세스와 메모리를 공유한다. 따라서 배치 결정에서는 모델 매개변수만이 아니라 최대 및 지속 메모리 사용량(peak and sustained memory usage)을 평가해야 한다.

전력 및 열 한계(power and thermal limits)는 계산적으로 가능해 보이는 배치 결정을 변경할 수 있다. 모델이 짧은 벤치마크에서는 온보드 GPU에서 정상적으로 실행되더라도 장시간 임무에서는 배터리 소모 또는 열 스로틀링(thermal throttling) 때문에 지속적인 실행이 불가능할 수 있다. 일부 작업을 로봇 외부로 이동시키면 지속적인 가속기 활용률을 낮추고 운용 시간을 연장하며, 환경 복잡도가 갑자기 증가했을 때 안전 중요 처리를 위한 열적 여유(thermal headroom)를 확보할 수 있다.

하드웨어 이질성(hardware heterogeneity)은 작업 분할을 더욱 복잡하게 만든다. CPU, GPU, NPU, 마이크로컨트롤러(microcontroller), 전용 가속기(specialized accelerator)는 서로 다른 장점, 메모리 아키텍처, 정밀도 지원(precision support), 스케줄링 특성을 가진다. 따라서 배치는 엣지, 온프레미스, 클라우드 사이에서만 이루어지는 것이 아니라 각 계층 내부의 프로세서 사이에서도 이루어진다. 완전한 작업 부하 맵(workload map)은 실행되는 물리적 위치와 실행을 담당하는 하드웨어 자원을 모두 지정해야 한다.

모델 최적화(model optimization)는 시간이 지나면서 이러한 경계를 이동시킬 수 있다. 양자화(quantization), 가지치기(pruning), 지식 증류(knowledge distillation), 희소 연산(sparse computation), 효율적인 어텐션(efficient attention), 하드웨어 인지 컴파일(hardware-aware compilation), 입력 해상도 감소를 통해 서버급 모델(server-class model)을 엣지 배포 가능 모델(edge-deployable model)로 변환할 수 있다. 가속기 성능 향상도 동일한 효과를 만들 수 있다. 따라서 모델 배치는 영구적인 아키텍처 결정으로 간주하지 말고 하드웨어와 소프트웨어가 발전함에 따라 재평가해야 한다.

캐싱(caching)은 상위 계층에 대한 의존성을 줄이는 또 다른 방법이다. 자주 사용하는 모델, 임베딩, 지도, 정책, 지식을 원래 클라우드에서 생성했더라도 온프레미스 또는 온보드에 저장할 수 있다. 로봇은 연결이 끊어진 상황에서도 캐시된 결과물(cached artifact)을 계속 사용하고 이후 새로운 버전을 동기화할 수 있다. 로컬 모델 저장소(local model repository)를 사용하면 모든 로봇이 광역 네트워크를 통해 동일한 자원을 반복적으로 다운로드하는 것도 방지할 수 있다.

여러 작업이 동일한 엣지 GPU 또는 온프레미스 클러스터를 공유하면 자원 경합(resource contention)을 고려해야 한다. 인식, 월드 모델링, 계획, 추론, 학습, 분석 작업이 가속기와 메모리를 두고 경쟁할 수 있다. 우선순위 인지 스케줄링(priority-aware scheduling)을 사용하면 안전 중요 작업을 보호하고, 운용 부하가 높을 때 선택적 추론이나 배치 처리(batch processing)를 양보시킬 수 있다. 따라서 배치는 단순한 정적 배포(static deployment) 결정이 아니라 런타임 오케스트레이션(runtime orchestration)과 연결된다.

개인정보 보호(privacy), 보안(security), 데이터 주권(data sovereignty)은 계산 관점에서 최적인 배치를 무시해야 하는 상황을 만들 수도 있다. 민감한 정보는 로봇, 시설, 조직 또는 특정 관할권(jurisdiction) 내부에 유지해야 할 수 있다. 이러한 경우 원격 인프라가 더 빠르거나 저렴하더라도 모델을 로컬에서 실행해야 할 수 있다. 또는 상위 계층으로 파생 표현(derived representation)을 전달하기 전에 전처리를 통해 민감한 내용을 제거할 수 있다. 따라서 배치 정책(placement policy)은 초기 단계부터 거버넌스 제약(governance constraint)을 포함해야 한다.

검증 요구사항(validation requirement) 역시 모델의 배포 위치에 영향을 준다. 새롭게 학습된 모델은 클라우드에서 생성될 수 있지만 평가 없이 안전 관련 로봇 운용에 직접 투입되어서는 안 된다. 클라우드 테스트를 통해 일반적인 성능을 확인하고, 온프레미스 검증을 통해 사이트별 행동(site-specific behavior)을 확인하며, 단계적 엣지 배포(staged edge deployment)를 통해 실제 하드웨어 제약에서의 동작을 검증할 수 있다. 새로운 버전이 예상과 다르게 동작할 경우 이전에 검증된 모델을 유지할 수 있도록 롤백 메커니즘(rollback mechanism)도 제공해야 한다.

따라서 실용적인 작업 분할 전략(partitioning strategy)은 지연시간, 안전 중요도, 연산 집약도, 메모리, 대역폭, 연결성, 작업 빈도, 에너지, 열 한계, 개인정보 보호, 공유 요구사항, 장애 시 동작(failure behavior)을 함께 고려한다. 하나의 지표만으로 올바른 위치를 결정할 수는 없다. 바람직한 배치는 불필요한 계산, 통신, 에너지 소비, 인프라 의존성을 최소화하면서 물리적 임무(physical mission)의 요구사항을 만족하는 구성이다.

결과적으로 이러한 아키텍처는 고정된 구조가 아니라 동적인 구조(dynamic architecture)가 된다. 모델과 작업은 임무 조건, 네트워크 품질, 하드웨어 가용성, 계산 수요의 변화에 따라 엣지, 온프레미스, 클라우드 자원 사이를 이동할 수 있다. 경량 로컬 지능(lightweight local intelligence)은 지속적인 자율성을 유지하고, 공유 인프라는 더 심층적인 운용 추론(deeper operational reasoning)을 제공하며, 글로벌 자원(global resource)은 대규모 학습을 담당한다. 따라서 모델 배치와 작업 부하 분할은 분산된 계산 자원을 하나의 일관된 물리 인공지능 시스템(coherent Physical AI system)으로 통합하는 핵심 메커니즘이 된다.

## 05.09. Data Upload and Model Download

![](images/image9.png){width="7.268055555555556in" height="7.268055555555556in"}

데이터 업로드(data upload)와 모델 다운로드(model download)는 운용 중인 로봇을 온프레미스(on-premise) 및 클라우드 지능(cloud intelligence)과 연결하는 양방향 정보 파이프라인(bidirectional information pipeline)을 형성한다. 물리 인공지능(Physical AI) 시스템은 엣지(edge)에서 지속적으로 경험을 생성하고, 대규모 인프라는 선택된 경험을 개선된 모델, 지도, 정책(policy), 지식으로 변환한다. 따라서 아키텍처는 로봇 운용을 방해하지 않으면서 유용한 데이터를 상위 계층으로 효율적으로 이동시키고 검증된 지능을 하위 계층으로 통제된 방식으로 전달할 수 있어야 한다.

로봇은 카메라(camera), 라이다(LiDAR), 레이더(radar), 깊이 센서(depth sensor), 관성측정장치(IMU), 마이크(microphone), 관절 엔코더(joint encoder), 내부 진단 정보(internal diagnostics), 응용 프로그램 로그(application log)로부터 막대한 양의 원시 정보(raw information)를 생성할 수 있다. 모든 데이터를 지속적으로 업로드하는 것은 일반적으로 불필요하고 비효율적이다. 엣지 시스템은 먼저 어떤 정보가 운용, 진단 또는 학습 가치를 가지는지 판단하여 중복된 관측이 아니라 유용한 물리 세계 경험에 통신 및 저장 자원을 집중해야 한다.

데이터 선택(data selection)은 단순한 연속 기록보다 이벤트(event)를 중심으로 수행할 수 있다. 예상하지 못한 장애물, 위치 추정 실패(localization failure), 낮은 신뢰도의 인식, 비정상적인 객체, 인간 개입(human intervention), 계획 실패(planning failure), 비상 정지(emergency stop), 예측 오류(prediction error), 새로운 환경 조건이 관련 센서 구간의 보존을 유발할 수 있다. 이벤트 전후의 데이터를 함께 기록하면 이후 로봇이 특정 행동을 수행한 이유를 분석하는 데 필요한 시간적 문맥(temporal context)을 확보할 수 있다.

일상적인 관측(routine observation) 역시 지능적으로 샘플링하면 가치 있는 학습 정보를 제공할 수 있다. 주기적 샘플(periodic sample)은 이벤트 트리거(event trigger)만으로는 놓칠 수 있는 점진적인 환경 변화, 새로운 운용 조건, 계절적 변화, 센서 열화(sensor degradation), 변화하는 인간 행동을 포착할 수 있다. 따라서 실용적인 데이터 파이프라인은 이벤트 기반 수집(event-driven collection)과 통제된 백그라운드 샘플링(background sampling)을 결합하여 어려운 사례와 정상 운용을 모두 표현하는 데이터셋을 생성한다.

업로드 전에 엣지 처리를 통해 필터링(filtering), 압축(compression), 특징 추출(feature extraction), 다운샘플링(downsampling), 요약(summarization)을 수행하여 데이터 양을 줄일 수 있다. 원시 이미지는 효율적으로 인코딩하고, 포인트 클라우드(point cloud)는 크롭(cropping)하거나 복셀화(voxelization)하며, 긴 텔레메트리 스트림(telemetry stream)은 통계 또는 선택된 구간으로 표현할 수 있다. 목표는 향후 분석에 필요한 정보를 보존하면서 불필요한 네트워크 트래픽, 저장공간 소비, 에너지 소비를 방지하는 것이다.

센서 데이터는 운용 문맥이 없으면 이후 해석하기 어려울 수 있으므로 메타데이터(metadata)가 필수적이다. 업로드 패키지(upload package)에는 타임스탬프(timestamp), 로봇 식별자(robot identifier), 소프트웨어 및 모델 버전, 센서 구성(sensor configuration), 임무 상태(mission state), 환경 조건, 신뢰도 값(confidence value), 개입 표시(intervention marker), 실패 코드(failure code)를 포함할 수 있다. 정확한 메타데이터를 통해 엔지니어와 학습 시스템은 관측이 생성된 조건을 재구성하고 서로 다른 모델 세대(model generation)의 행동을 비교할 수 있다.

온프레미스 인프라(on-premise infrastructure)는 로봇 데이터의 첫 번째 통합 지점(aggregation point)으로 동작할 수 있다. 여러 로봇은 원격 클라우드 서비스와 개별적으로 통신하는 대신 고대역폭 로컬 네트워크를 통해 선택된 경험을 업로드할 수 있다. 로컬 서버는 데이터를 버퍼링(buffering)하고, 무결성을 검증하며, 중복을 제거하고, 멀티모달 스트림(multimodal stream)을 동기화하고, 이벤트를 색인화하며, 개인정보 필터(privacy filter)를 적용하고, 어떤 정보를 로컬에 유지하고 어떤 정보를 상위 계층으로 전달할지 결정하기 전에 데이터셋으로 구성할 수 있다.

통신 가용성(communication availability)을 항상 보장할 수 없으므로 버퍼링은 중요하다. 로봇은 무선 통신 범위가 불안정한 영역에서 운용되거나 로컬 인프라와의 연결을 일시적으로 잃을 수 있다. 업로드 큐(upload queue)는 연결이 복구될 때까지 선택된 데이터를 보관할 수 있으며, 우선순위 정책(priority policy)은 어떤 정보를 먼저 전송할지 결정한다. 대역폭이 일시적으로 제한되면 중요한 고장 기록이나 희귀 이벤트가 일상적인 텔레메트리보다 높은 우선순위를 받을 수 있다.

데이터 무결성(data integrity)은 전체 전송 과정에서 검증되어야 한다. 체크섬(checksum), 시퀀스 식별자(sequence identifier), 트랜잭션 기록(transaction record), 재개 가능한 전송(resumable transfer)을 사용하면 손상되거나 불완전한 업로드가 학습 데이터셋에 조용히 포함되는 것을 방지할 수 있다. 대규모 센서 파일은 통신이 중단될 때마다 처음부터 다시 전송할 필요가 없어야 한다. 이러한 신뢰성 높은 전송 메커니즘은 로봇이 장시간 동작하면서 대규모 분산 데이터를 생성하는 환경에서 특히 중요하다.

클라우드는 여러 온프레미스 사이트에서 큐레이션된 정보(curated information)를 수신하여 플릿 규모 데이터셋(fleet-scale dataset)으로 통합할 수 있다. 서로 다른 로봇, 임무, 환경, 지리적 위치의 데이터는 단일 시설에서는 발견하기 어려운 실패 패턴과 운용 조건을 보여줄 수 있다. 중앙 통합(central aggregation)은 대규모 분석, 시뮬레이션, 모델 평가, 학습을 지원하며, 동시에 로컬 인프라는 사이트별 정보와 운용 서비스를 계속 관리한다.

따라서 상향 파이프라인(upward pipeline)은 상위 계층으로 이동할수록 점점 더 선택적이어야 한다. 로봇은 원시 물리 경험(raw physical experience)을 생성하고, 엣지는 관련 구간을 선택하며, 온프레미스 시스템은 이를 통합하고 큐레이션하고, 클라우드 시스템은 장기적인 학습 또는 조직적 가치가 있는 정보를 보존한다. 데이터가 위로 이동할수록 전체 데이터 양은 감소하는 반면 의미론적 가치(semantic value)는 증가할 수 있다. 이러한 계층 구조는 중앙 인프라가 모든 센서 측정값을 수동적으로 저장하는 거대한 저장소가 되는 것을 방지한다.

개인정보 보호(privacy)와 데이터 주권(data sovereignty)은 이러한 필터링 과정에 영향을 주어야 한다. 사람의 이미지, 시설 배치(facility layout), 산업 공정, 오디오 또는 독점적인 운용 정보는 로봇이나 로컬 사이트 외부로 이동하는 것이 허용되지 않을 수 있다. 민감한 영역은 제거하거나 익명화(anonymization)하거나 특징으로 변환하거나 온프레미스에만 유지할 수 있다. 클라우드는 제한되지 않은 원시 데이터 대신 승인된 표현과 통계를 수신함으로써 거버넌스 요구사항(governance requirement)을 준수하면서 학습 가치를 유지할 수 있다.

데이터가 통합되면 모델 개선(model improvement)에 사용할 수 있다. 어려운 사례를 레이블링(labeling)하거나 자동으로 구성하고, 실패 에피소드(failure episode)를 재생하며, 새로운 학습 데이터셋을 구축할 수 있다. 클라우드 또는 온프레미스 인프라는 인식 네트워크(perception network), 월드 모델(world model), 정책, 멀티모달 시스템(multimodal system), 작업 특화 어댑터(task-specific adapter)를 학습할 수 있다. 따라서 상향 데이터 파이프라인의 출력은 결국 개선된 지능을 생성하는 학습 프로세스의 입력이 된다.

모델 다운로드(model download)는 이러한 순환의 반대 방향을 형성한다. 새롭게 학습된 모델은 단순히 학습이 완료되었다는 이유만으로 실제 운용 로봇에 직접 전송되어서는 안 된다. 후보 모델(candidate model)은 평가(evaluation), 버전 관리(versioning), 호환성 검사(compatibility check), 안전 검증(safety validation), 배포 승인(deployment approval)을 거쳐야 한다. 따라서 하향 파이프라인(downward pipeline)은 모델 파일을 일반 데이터처럼 취급하는 것이 아니라 통제된 소프트웨어 결과물(controlled software artifact)로 관리해야 한다.

모델 패키지(model package)는 신경망 가중치(neural-network weights) 이상의 정보를 포함할 수 있다. 런타임 설정(runtime configuration), 전처리 매개변수(preprocessing parameter), 보정 정보(calibration information), 지원되는 센서 버전, 추론 엔진 요구사항(inference-engine requirement), 하드웨어 호환성, 예상 메모리 사용량, 안전 제약조건, 배포 메타데이터를 포함할 수 있다. 이러한 의존성을 하나의 패키지로 구성하면 유효한 모델이 호환되지 않는 소프트웨어나 센서 설정 때문에 잘못 동작할 위험을 줄일 수 있다.

온프레미스 인프라는 로컬 모델 저장소(local model repository)이자 배포 게이트웨이(deployment gateway) 역할을 할 수 있다. 모든 로봇이 클라우드에서 대규모 파일을 독립적으로 다운로드하는 대신 검증된 모델을 시설로 한 번 전송한 뒤 로컬에서 캐싱(caching)할 수 있다. 이후 로봇은 로컬 네트워크를 통해 적절한 버전을 가져온다. 이를 통해 광역 네트워크 대역폭 사용을 줄이고 플릿 배포 속도를 높이며 사이트 운영자가 업데이트가 실제 운용 장비에 적용되는 시점을 통제할 수 있다.

모델 배포(model distribution)는 일반적으로 전체 시스템에 동시에 수행하기보다 단계적으로 수행해야 한다. 새로운 모델을 먼저 시뮬레이션, 테스트 로봇(test robot), 또는 실제 운용 플릿의 일부에 배포할 수 있다. 이후 성능을 모니터링한 다음 배포 범위를 확대한다. 카나리 배포(canary deployment), 단계적 롤아웃(phased rollout), 통제된 활성화(controlled activation)를 통해 예상하지 못한 모델의 취약점이 동일한 소프트웨어 세대를 사용하는 모든 로봇에 즉시 영향을 미치는 위험을 줄일 수 있다.

원자적 활성화(atomic activation)는 불완전한 업데이트가 실제 운용에 영향을 주는 것을 방지한다. 로봇은 새로운 패키지를 비활성 저장공간(inactive storage)에 다운로드하고, 무결성과 호환성을 검증한 뒤, 필요한 모든 구성요소가 준비되었을 때만 활성화할 수 있다. 현재 검증된 모델은 전송 중에도 계속 동작한다. 설치가 실패하면 부분적으로 업데이트된 지능을 사용하는 불명확한 상태(undefined state)에 진입하지 않고 기존 버전을 계속 사용할 수 있다.

롤백 기능(rollback capability) 역시 중요하다. 오프라인 평가에서 우수한 성능을 보인 모델이라도 실제 배포 이후 예상하지 못한 조건을 만날 수 있다. 로봇은 이전에 검증된 모델을 유지하거나 신뢰할 수 있는 다른 복구 메커니즘을 제공해야 한다. 모니터링을 통해 허용할 수 없는 정확도, 지연시간, 자원 소비, 불안정성, 안전 관련 행동이 감지되면 새로운 모델을 조사하는 동안 기존의 검증된 구성으로 복귀할 수 있다.

다운로드는 운용 시점(operational timing)도 고려해야 한다. 로봇이 통신 집약적인 임무를 수행하는 동안 수 기가바이트 규모의 모델을 전송하면 센서 트래픽이나 플릿 협업에 영향을 줄 수 있다. 업데이트는 충전, 유지보수, 유휴 시간(idle period), 네트워크 사용량이 낮은 시간에 수행하도록 예약할 수 있다. 대역폭 조절(bandwidth shaping)과 전송 우선순위 설정(transfer prioritization)을 통해 모델 생애주기 작업과 실시간 로봇 통신을 함께 운영할 수 있다.

증분 업데이트(incremental update)는 전송 요구량을 더욱 줄일 수 있다. 모델, 설정, 지도, 지식베이스(knowledge base)의 일부만 변경되었다면 전체 결과물 대신 차이만 델타 패키지(delta package)로 전송할 수 있다. 모델 캐싱(model caching), 공유 계층(shared layer), 재사용 가능한 임베딩(reusable embedding), 로컬 저장소 역시 반복적인 전송을 줄일 수 있다. 이러한 기법은 플릿 규모가 커지고 모델 크기가 증가할수록 더욱 중요해진다.

보안(security)은 파이프라인의 양방향 모두를 보호해야 한다. 업로드되는 데이터는 인증(authentication)과 암호화(encryption)를 적용하여 운용 정보가 가로채이거나 변경되지 않도록 해야 한다. 다운로드된 모델은 승인되지 않은 지능이 로봇에 들어가는 것을 방지하기 위해 활성화 전에 서명(signing)과 검증(verification)을 수행해야 한다. 신원 관리(identity management), 접근 제어(access control), 감사 로그(audit log), 키 관리(key management), 출처 추적(provenance tracking)은 데이터 생성부터 모델 배포까지 신뢰 체계(trust chain)를 형성하는 데 도움이 된다.

버전 추적성(version traceability)은 업로드된 경험과 다운로드된 지능 사이의 관계를 완성한다. 엔지니어는 특정 실패 기록을 어떤 모델이 생성했는지, 해당 기록이 어떤 데이터셋에 포함되었는지, 어떤 학습 프로세스가 그 데이터셋을 사용했는지, 그리고 그 결과 어떤 새로운 모델이 생성되었는지를 추적할 수 있어야 한다. 이러한 계보(lineage)는 물리 세계의 이벤트를 모델 개발 과정과 연결하며 이후 버전의 동작이 달라졌을 때 회귀 분석(regression analysis)을 가능하게 한다.

따라서 전체 파이프라인은 지속적 학습 사이클(continuous learning cycle)을 형성한다. 물리적 운용은 경험을 생성하고, 유용한 경험은 상위 계층으로 이동하며, 대규모 인프라는 이를 큐레이션하고 학습한다. 검증된 지능은 다시 하위 계층으로 이동하고, 업데이트된 로봇은 새로운 경험을 생성한다. 각 반복 과정은 추가적인 어려운 사례를 발견하고 이후 모델을 개선할 수 있다. 따라서 데이터 전송은 단순한 인프라 기능이 아니라 물리 인공지능의 학습 아키텍처(learning architecture)를 구성하는 핵심 요소이다.

잘 설계된 업로드 및 다운로드 시스템은 서로 경쟁하는 여러 요구사항 사이의 균형을 유지해야 한다. 네트워크에 과부하를 주지 않으면서 가치 있는 경험을 보존하고, 개인정보 보호를 위반하지 않으면서 대규모 학습을 가능하게 하며, 임무 수행을 중단하지 않으면서 개선된 모델을 배포하고, 통신을 사용할 수 없을 때에도 안전한 자율성(safe autonomy)을 유지해야 한다. 엣지 선택(edge selection), 로컬 통합(local aggregation), 글로벌 학습(global learning), 검증(validation), 단계적 배포(staged deployment), 롤백을 조정함으로써 물리 인공지능은 분산된 로봇 경험을 지속적으로 개선되는 운용 지능(operational intelligence)으로 변환할 수 있다.

## 05.10. Intermittent Connectivity and Offline Autonomy

![](images/image10.png){width="7.268055555555556in" height="7.268055555555556in"}

간헐적 연결성(intermittent connectivity)은 물리 인공지능(Physical AI)에서 예외적인 네트워크 장애가 아니라 정상적인 운용 조건이다. 모바일 로봇은 창고, 공장, 터널, 실외 지형, 재난 지역, 지하시설 또는 원격 환경을 이동하면서 무선 통신 범위가 지속적으로 변화할 수 있다. 따라서 강건한 아키텍처(robust architecture)는 대역폭(bandwidth), 지연시간(latency), 가용성(availability)이 변동하며 외부 컴퓨팅 서비스를 주기적으로 사용할 수 없게 될 수 있다는 것을 기본적으로 가정해야 한다.

오프라인 자율성(offline autonomy)은 로봇이 온프레미스(on-premise) 또는 클라우드 연결에 의존하지 않고 안전한 운용을 지속하는 데 필요한 최소한의 지능을 유지하는 것을 의미한다. 필수적인 인식(perception), 위치 추정(localization), 상태 추정(state estimation), 장애물 회피(obstacle avoidance), 로컬 계획(local planning), 안전 모니터링(safety monitoring), 액추에이터 제어(actuator control)는 온보드에서 계속 사용할 수 있어야 한다. 외부 인프라는 기능을 향상시킬 수 있지만 네트워크 연결이 끊어졌다고 해서 기본적인 물리적 안전과 운용 안정성이 사라져서는 안 된다.

이러한 요구사항은 필수 자율성(essential autonomy)과 향상된 지능(enhanced intelligence) 사이에 명확한 아키텍처 경계를 형성한다. 즉각적인 물리적 상호작용을 직접 담당하는 기능은 엣지(edge)에 위치하고, 계산 비용이 높은 추론, 플릿 전체 최적화(fleet-wide optimization), 대규모 월드 모델(world model), 분석(analytics), 학습(training)은 로봇 외부에 위치할 수 있다. 따라서 연결성은 로봇이 안전하게 기능할 수 있는지를 결정하는 요소가 아니라 로봇이 수행할 수 있는 능력을 확장하는 요소가 되어야 한다.

네트워크 상태(network condition)는 동적인 시스템 상태(dynamic system state)로 취급해야 한다. 로봇은 연결 가용성, 왕복 지연시간(round-trip latency), 패킷 손실(packet loss), 대역폭, 큐 깊이(queue depth), 서비스 응답성(service responsiveness)을 지속적으로 관측할 수 있다. 이러한 측정값을 이용하면 자율 시스템은 원격 기능이 현재 작업에서 사용할 만큼 충분히 신뢰할 수 있는지를 판단할 수 있다. 이에 따라 연산 오프로딩(computation offloading) 결정은 영구적인 연결을 가정하지 않고 실제 통신 품질에 맞게 적응할 수 있다.

서로 다른 연결 상태에 따라 서로 다른 운용 모드(operational mode)를 구성할 수 있다. 연결 상태가 양호하면 로봇은 엣지, 온프레미스, 클라우드 기능을 함께 사용할 수 있다. 연결성이 저하되면 로컬 서비스를 우선하고 선택적인 데이터 전송을 줄일 수 있다. 완전히 오프라인 상태가 되면 통신을 다시 사용할 수 있을 때까지 온보드 모델, 캐시된 지도(cached map), 로컬 정책(local policy), 이전에 동기화된 지식에 의존하여 동작할 수 있다.

이러한 전환 과정에서는 점진적 성능 저하(graceful degradation)가 필수적이다. 서버에 연결할 수 없다는 이유만으로 시스템이 완전한 기능 상태에서 즉시 전체 장애 상태로 전환되어서는 안 된다. 선택적인 의미론적 추론(semantic reasoning), 원격 분석(remote analytics), 전역 최적화(global optimization), 비필수 모델 서비스부터 비활성화할 수 있다. 로봇은 중요한 인식과 제어 기능을 유지하면서 운용 중요도와 연결 의존성에 따라 기능을 점진적으로 축소할 수 있다.

로컬 캐싱(local caching)은 오프라인 운용을 위한 중요한 기반을 제공한다. 자주 필요한 지도, 내비게이션 정보, 임무 지시(mission instruction), 모델, 정책, 보정 매개변수(calibration parameter), 의미론적 지식(semantic knowledge), 설정 파일(configuration file)을 온보드에 저장할 수 있다. 예측 가능하고 재사용할 수 있는 정보를 얻기 위해 로봇이 매번 원격 서버에 접속할 필요는 없다. 캐시된 자원을 이용하면 일시적인 통신 장애 중에도 정상적인 작업을 계속 수행할 수 있다.

그러나 저장된 정보는 시간이 지나면서 오래될 수 있으므로 캐시(cache)를 신중하게 관리해야 한다. 각 결과물(artifact)에는 버전(version), 타임스탬프(timestamp), 유효 기간(validity period), 호환성 요구사항(compatibility requirement), 출처 식별자(source identifier)를 포함할 수 있다. 로봇은 오프라인에서도 안전하게 사용할 수 있는 자원과 특정 작업을 계속하기 전에 동기화가 필요한 자원을 구분할 수 있다. 이를 통해 오래된 지도, 구형 정책, 호환되지 않는 설정이 물리적 행동에 영향을 미치는 것을 방지할 수 있다.

임무 계획(mission planning)은 연결성 제약(connectivity constraint)을 명시적으로 고려해야 한다. 로봇이 이동 경로의 일부가 통신 음영 지역을 통과한다는 사실을 알고 있다면 해당 구간에 진입하기 전에 필요한 모델, 지도, 명령, 지식을 미리 로드(preload)할 수 있다. 이러한 예측적 준비(predictive preparation)는 연결 손실을 예상하지 못한 장애가 아니라 계획된 운용 조건으로 전환하고 비상 복구 메커니즘에 대한 의존성을 줄인다.

오프라인 상태에서 생성된 데이터는 우선순위와 사용 가능한 저장공간에 따라 로컬에 보존해야 한다. 안전 이벤트(safety event), 장애, 인간 개입(human intervention), 비정상적인 관측, 진단 정보(diagnostic information), 학습 관련 경험은 즉시 업로드할 수 없더라도 보존할 필요가 있다. 로컬 큐(local queue)는 연결이 복구될 때까지 이러한 기록을 정리하여 일시적인 통신 손실이 운용 기록의 영구적인 공백으로 이어지는 것을 방지할 수 있다.

저장공간의 한계 때문에 선택적 보존(selective retention)이 필요하다. 수 시간 또는 수 일 동안 오프라인으로 동작하는 로봇은 저장할 수 있는 양보다 훨씬 많은 센서 데이터를 생성할 수 있다. 이벤트 기반 기록(event-driven recording), 압축(compression), 요약(summarization), 다운샘플링(downsampling), 우선순위 기반 삭제(priority-based deletion)를 이용해 가장 가치 있는 정보를 보호할 수 있다. 저장공간이 부족해질 경우 중요한 장애 증거와 희귀한 학습 사례를 유지하면서 반복적인 일상 관측은 제거할 수 있다.

반대 방향으로 이동하는 명령(command)도 유사하게 처리해야 한다. 원격 임무 명령, 지도 업데이트(map update), 정책 변경(policy change), 모델 배포(model deployment)는 로봇의 연결이 끊어진 동안 지연될 수 있다. 시스템은 재연결 이후에도 유효한 명령과 운용 문맥이 이미 만료된 명령을 구분해야 한다. 타임스탬프, 버전 관리(versioning), 만료 규칙(expiration rule), 임무 상태 검사(mission-state check)를 통해 지연된 명령이 부적절한 상황에서 실행되는 것을 방지할 수 있다.

연결이 복구되면 동기화(synchronization)는 즉각적이고 무제한적으로 수행하는 것이 아니라 통제된 방식으로 이루어져야 한다. 로봇에는 오프라인 동안 축적된 센서 기록, 로그, 지도 변경사항, 상태 업데이트가 존재할 수 있으며, 동시에 외부 시스템에는 새로운 모델, 정책 또는 임무 정보가 존재할 수 있다. 조정 메커니즘(reconciliation mechanism)은 양쪽에서 무엇이 변경되었는지를 판단하고 정상적인 연결 운용을 재개하기 전에 적절한 전송 순서를 결정해야 한다.

재연결 직후에는 대기 중이던 트래픽이 갑자기 집중될 수 있으므로 우선순위 설정(prioritization)이 특히 중요하다. 안전 사고, 장애 로그, 임무 중요 상태(mission-critical state), 동기화 메타데이터를 일상적인 텔레메트리(telemetry)보다 먼저 업로드할 수 있다. 대규모 선택적 모델보다 필수적인 설정 변경을 먼저 다운로드할 수 있다. 대역폭 조절(bandwidth shaping)과 큐 스케줄링(queue scheduling)은 복구 트래픽이 현재 진행 중인 로봇 운용에 필요한 실시간 통신을 방해하는 것을 방지한다.

재개 가능한 전송(resumable transfer) 메커니즘은 연결과 단절이 반복되는 환경을 지원한다. 대규모 데이터셋이나 모델 패키지는 네트워크가 끊어질 때마다 처음부터 다시 시작하는 대신 마지막으로 검증된 구간부터 전송을 계속해야 한다. 청킹(chunking), 체크섬(checksum), 시퀀스 추적(sequence tracking), 승인 응답(acknowledgment), 영구 전송 상태(persistent transfer state)를 이용하면 여러 번의 짧은 연결 구간을 통해서도 통신 진행 상태를 누적할 수 있다.

분산 플릿 운용(distributed fleet operation)은 추가적인 문제를 발생시킨다. 로봇은 중앙 플릿 관리자(central fleet manager)와의 연결을 잃더라도 주변 로봇과는 통신 가능한 상태를 유지할 수 있다. 시스템 아키텍처에 따라 피어 투 피어 통신(peer-to-peer communication)을 이용해 제한적인 협업, 공유 관측(shared observation), 로컬 작업 협상(local task negotiation), 비상 정보 교환을 수행할 수 있다. 이러한 메커니즘은 중앙 인프라를 일시적으로 사용할 수 없더라도 유용한 집단 행동(collective behavior)을 유지할 수 있도록 한다.

그러나 오프라인 플릿 협업(offline fleet coordination)을 위해서는 명확한 권한(authority)과 충돌 관리 규칙(conflict-management rule)이 필요하다. 두 로봇이 서로 오래된 작업 할당 정보를 가지고 있거나 공유 자원에 대해 경쟁하는 결정을 독립적으로 내릴 수 있다. 로컬 정책은 전역 협업을 사용할 수 없는 상황에서 예약 행동(reservation behavior), 통행 우선권(right-of-way), 작업 소유권(task ownership), 충돌 해결(conflict resolution)을 정의해야 한다. 재연결 이후에는 로컬에서 내려진 결정과 권한을 가진 플릿 상태(authoritative fleet state)를 다시 조정해야 한다.

위치 추정(localization) 역시 보정 서비스나 중앙집중형 지도를 사용하는 경우 연결성에 의존할 수 있다. 로봇은 위성항법시스템 보정(GNSS correction), 지도 서버(map server), 외부 랜드마크(external landmark), 공유 위치 추정 서비스(shared localization service)가 사라질 때 위치 추정 불확실성이 어떻게 변화하는지 이해해야 한다. 동시에 불확실성을 모니터링하면서 온보드 비전(vision), 라이다, 관성(inertial), 오도메트리(odometry) 기반 추정에 더 많이 의존하고 그에 맞게 운용 범위(operating envelope)를 조정할 수 있다.

불확실성(uncertainty)은 오프라인 행동에 직접적인 영향을 주어야 한다. 위치 추정 신뢰도가 감소하거나 환경적 모호성(environmental ambiguity)이 증가하면 로봇은 속도를 낮추고, 장애물 안전 여유(obstacle margin)를 확대하며, 계획 구간(planning horizon)을 단축하고, 어려운 영역을 회피하거나, 불확실성이 안전 임계값(safe threshold)을 초과하기 전에 정지할 수 있다. 따라서 오프라인 자율성은 모든 조건에서 동일한 성능을 유지해야 한다는 의미가 아니라 기능이 예측 가능하고 안전한 방식으로 저하되어야 한다는 의미이다.

월드 모델(world model)은 로컬에서 사용할 수 있는 정보만으로 로봇이 얼마나 오랫동안 신뢰성 있게 운용할 수 있는지를 예측함으로써 이러한 적응형 행동(adaptive behavior)을 지원할 수 있다. 최근 관측, 캐시된 환경 지식, 움직임 추정(motion estimate), 불확실성 전파(uncertainty propagation)는 통신 손실 중 일시적인 예측 표현(predictive representation)을 제공할 수 있다. 예측의 신뢰성이 감소하면 자율 시스템은 오래된 정보가 계속 정확하다고 가정하는 대신 점차 더 보수적인 행동으로 전환할 수 있다.

안전 중요 기능(safety-critical function)은 원격 응답을 무한정 기다려서는 안 된다. 운용 중 사용되는 모든 외부 요청에는 명확한 타임아웃(timeout)과 폴백 행동(fallback behavior)이 정의되어야 한다. 원격 플래너(remote planner), 의미론적 추론기(semantic reasoner), 플릿 서비스(fleet service)가 유효한 시간 범위 내에 응답하지 않으면 로봇은 적절한 로컬 대안을 사용하여 계속 동작해야 한다. 늦게 도착한 응답이 단순히 최종적으로 수신되었다는 이유로 더 최신의 물리 상태를 덮어써서는 안 된다.

재연결 과정에서도 보안(security)은 중요하다. 장시간 오프라인 상태였던 로봇은 저장된 운용 데이터를 업로드하는 동시에 축적된 업데이트와 명령을 수신할 수 있다. 빠른 동기화가 필요하더라도 인증(authentication), 암호화(encryption), 버전 검증(version validation), 서명(signature), 권한 검사(authorization check)는 계속 유지되어야 한다. 편의를 위해 정상적인 신뢰 메커니즘(trust mechanism)을 우회하는 일시적인 구간이 재연결 과정에서 발생해서는 안 된다.

오프라인 자율성은 소프트웨어 및 모델 배포 전략(software and model deployment strategy)에도 영향을 미친다. 로봇은 서버에서 언제든 다시 가져올 수 있다고 가정하는 대신 정상 동작이 검증된 모델(known-good model)과 필요한 런타임 의존성(runtime dependency)을 로컬에 유지해야 한다. 새로운 업데이트는 비활성 저장공간(inactive storage)에 다운로드하고 완전한 검증 이후에만 활성화할 수 있다. 전송 도중 연결이 끊어지더라도 기존에 검증된 구성을 유지하여 로봇이 계속 동작할 수 있어야 한다.

테스트(testing)는 이상적인 네트워크 환경에서만 자율성을 검증하는 것이 아니라 현실적인 통신 장애를 재현해야 한다. 엔지니어는 의도적으로 지연시간 증가, 대역폭 감소, 패킷 손실, 서버 사용 불가(server unavailability), 부분 전송(partial transfer), 반복적인 연결 해제, 장시간 오프라인 상태를 발생시켜야 한다. 목표는 개별 네트워크 구성요소가 복구되는지만 확인하는 것이 아니라 장애가 지속되는 전체 과정에서 로봇의 물리적 행동이 안전하고 예측 가능한지를 검증하는 것이다.

유용한 평가 지표(evaluation metric)에는 오프라인에서 유지되는 임무 기능의 비율, 연결 모드 사이의 전환 시간(transition time), 허용 가능한 최대 통신 중단 시간(maximum tolerable outage duration), 동기화 복구 시간(synchronization recovery time), 대기 데이터 손실(queued data loss), 오래된 정보 처리(stale-information handling), 통신 저하 상태에서의 안전 성능이 포함된다. 이러한 지표는 연결성을 독립적인 정보기술 특성으로 다루는 대신 네트워크 동작을 실제 자율 성능과 연결한다.

간헐적 연결성과 오프라인 자율성은 궁극적으로 로컬 우선 아키텍처(local-first architecture)를 요구한다. 엣지는 즉각적인 물리적 상호작용에 필요한 지능을 유지하고, 온프레미스 및 클라우드 시스템은 통신이 허용되는 동안 추가적인 추론, 협업, 학습, 계산 확장성(computational scale)을 제공한다. 캐싱, 버퍼링, 적응형 오프로딩(adaptive offloading), 불확실성 관리, 점진적 성능 저하, 통제된 동기화를 통해 이러한 계층은 로봇을 불안정하게 만들지 않으면서 분리되었다가 다시 연결될 수 있다.

따라서 잘 설계된 물리 인공지능 시스템은 연결성을 영구적인 전제 조건이 아니라 가변적인 자원(variable resource)으로 취급한다. 강력한 네트워크는 더욱 풍부한 공유 지능(shared intelligence)을 가능하게 하고, 약한 네트워크는 원격 의존성을 선택적으로 감소시키며, 완전한 연결 단절은 자율적인 로컬 운용(autonomous local operation)을 활성화한다. 통신이 복구되면 축적된 경험과 업데이트된 지능을 안전하게 동기화함으로써 로봇은 운용 연속성(operational continuity)과 물리적 안전(physical safety)을 유지하면서 연결된 세계와 단절된 세계 사이를 지속적으로 이동할 수 있다.

## 05.11. Fleet Intelligence and Shared Models

![](images/image11.png){width="7.268055555555556in" height="7.268055555555556in"}

플릿 지능(fleet intelligence)은 물리 인공지능(Physical AI)을 단일 로봇의 자율성에서 여러 로봇이 하나의 집단 시스템으로 협력하는 조정된 지능(coordinated intelligence)으로 확장한다. 각 로봇은 환경의 제한된 일부만 관측하지만, 플릿은 관측 정보, 운용 이력, 지도, 작업 상태, 학습된 경험을 결합할 수 있다. 공유 모델(shared model)은 한 기계가 획득한 지식을 다른 기계에서도 활용할 수 있도록 하여, 서로 독립된 자율 에이전트(autonomous agent)를 분산 학습 및 의사결정 시스템(distributed learning and decision system)으로 전환한다.

개별 로봇은 여전히 로컬 자율성(local autonomy)을 유지해야 한다. 플릿 지능이 즉각적인 물리적 상호작용에 필요한 실시간 기능을 대체할 수 없기 때문이다. 인식(perception), 위치 추정(localization), 장애물 회피(obstacle avoidance), 단기 계획(short-horizon planning), 안전 모니터링(safety monitoring), 액추에이터 제어(actuator control)는 주로 온보드에서 유지된다. 플릿 지능은 이러한 로컬 계층 위에서 동작하면서 기본적인 안전을 위한 필수 의존성이 되지 않고 각 로봇의 능력을 향상시키는 광범위한 문맥(context), 공유 지식, 협업, 학습을 제공한다.

온프레미스 인프라(on-premise infrastructure)는 사이트 수준 플릿 지능(site-level fleet intelligence)을 구현하기에 자연스러운 위치를 제공한다. 동일한 창고, 공장, 캠퍼스, 병원 또는 실외 시설에서 운용되는 로봇은 선택된 상태 정보를 로컬 서버에 지속적으로 전달할 수 있다. 이러한 서버는 로봇 위치, 임무 진행 상태, 교통 상황, 자원 가용성(resource availability), 환경 변화, 충전 상태, 공유 지도 등을 포함하는 보다 광범위한 운용 상황을 유지할 수 있으며, 이는 하나의 로봇이 완전하게 관측하기 어려운 정보이다.

작업 할당(task allocation)은 플릿 지능의 기본적인 기능 중 하나이다. 임무를 개별적으로 할당하는 대신 플릿 관리자(fleet manager)는 로봇 위치, 능력(capability), 배터리 수준, 작업 부하, 적재량(payload), 경로 비용(route cost), 유지보수 상태, 작업 우선순위를 함께 고려할 수 있다. 시스템은 사용 가능한 기계 사이에 작업을 분배하고 조건이 변화하면 작업을 다시 할당할 수 있다. 이를 통해 여러 자율 로봇의 집합을 플릿 전체의 수요를 균형 있게 처리할 수 있는 협력적 운용 자원(coordinated operational resource)으로 전환할 수 있다.

경로 협업(route coordination) 역시 공유 정보의 이점을 얻는다. 한 로봇은 로컬에서 유효한 경로를 계산할 수 있지만 다른 로봇이 동일한 좁은 통로나 교차로로 접근하고 있다는 사실을 알지 못할 수 있다. 플릿 수준 교통 관리(fleet-level traffic management)는 로컬 플래너가 이러한 상황에 직접 직면하기 전에 상호작용을 식별할 수 있다. 공유 예약(shared reservation), 경로 우선순위, 혼잡도 추정(congestion estimate), 통행 우선권 정책(right-of-way policy)을 이용하면 공용 인프라에서 교착 상태(deadlock), 불필요한 대기, 비효율적인 이동을 줄일 수 있다.

공유 지도(shared map)는 한 로봇이 발견한 환경 지식을 다른 로봇이 즉시 활용할 수 있도록 한다. 한 로봇이 차단된 통로, 변경된 바닥 상태, 임시 장애물, 공사 구역 또는 새롭게 주행 가능한 영역을 발견하면 해당 관측을 검증한 뒤 공통 지도(common map)에 반영할 수 있다. 다른 로봇은 동일한 조건을 물리적으로 다시 발견하지 않고도 행동을 변경할 수 있으므로 중복 탐색(redundant exploration)을 줄이고 플릿 전체의 적응 능력을 향상시킬 수 있다.

공유 표현(shared representation)은 기하학적 지도(geometric map)를 넘어 확장될 수 있다. 플릿 월드 메모리(fleet world memory)는 의미론적 객체(semantic object), 주행 가능성(traversability), 점유 상태(occupancy), 교통 통계, 인간 활동 패턴(human activity pattern), 위험 영역, 도킹 위치(docking location), 충전 자원, 시간에 따른 변화 등을 포함할 수 있다. 현재 상태와 과거 정보를 모두 보존함으로써 플릿은 개별 로봇이 자신의 제한된 관측만으로 유지할 수 있는 것보다 훨씬 풍부한 환경 운용 모델(operational model)을 구축할 수 있다.

공유 모델(shared model)은 동일한 원리를 환경 지식에서 학습된 지능(learned intelligence)으로 확장한다. 여러 로봇에서 수집된 어려운 관측을 이용해 개선된 인식 모델은 이후 전체 플릿에 다시 배포할 수 있다. 마찬가지로 월드 모델(world model), 내비게이션 정책(navigation policy), 이상 탐지기(anomaly detector), 작업 플래너(task planner), 의미론적 표현도 집단 경험(collective experience)을 이용해 학습할 수 있다. 따라서 추론이 개별 로봇에 분산되어 있더라도 학습은 플릿 수준의 프로세스가 될 수 있다.

여기에는 공유 모델 개발(shared model development)과 공유 모델 실행(shared model execution) 사이의 중요한 차이가 존재한다. 공통 모델은 플릿 데이터로 학습한 뒤 모든 로봇에서 독립적으로 실행할 수 있다. 다른 모델은 온프레미스 서버에 유지하면서 공유 추론 서비스(shared inference service)를 제공할 수 있다. 일부 시스템은 소형 버전을 온보드에 배포하고 대형 버전을 중앙에서 실행하는 두 방식을 동시에 사용할 수 있다. 적절한 구성은 지연시간(latency), 안전성, 연산 능력, 연결성(connectivity), 모델 크기에 따라 결정된다.

플릿 데이터는 학습 경험의 다양성(diversity)을 크게 증가시킬 수 있다. 서로 다른 로봇은 서로 다른 객체, 경로, 조명 조건, 노면 특성, 인간 행동, 장애, 비정상적인 이벤트를 경험한다. 이러한 관측을 통합하면 하나의 로봇이 수집할 수 있는 것보다 훨씬 광범위한 데이터셋을 구축할 수 있다. 하나의 기계에서는 드물게 발생하는 희귀 이벤트(rare event)도 수십, 수백 또는 수천 대의 로봇 경험을 축적하면 통계적으로 의미 있는 사례가 될 수 있다.

그러나 플릿 학습(fleet learning)은 모든 관측을 동일한 가치로 취급해서는 안 된다. 로봇은 낮은 신뢰도의 예측, 위치 추정 실패, 인간 개입(human intervention), 예상하지 못한 장애물, 계획 실패, 새로운 객체, 비정상적인 환경 변화를 식별할 수 있다. 이러한 이벤트에는 업로드와 학습에서 더 높은 우선순위를 부여할 수 있다. 공유 인프라는 이후 중복 데이터를 제거하고, 유사한 사례를 클러스터링(clustering)하며, 반복적으로 발생하는 플릿 수준의 실패 패턴을 식별하고, 실질적인 개선 가능성이 높은 사례를 중심으로 데이터셋을 구성할 수 있다.

집단 학습(collective learning)은 정책 개선(policy improvement)도 지원할 수 있다. 로봇은 다양한 운용 조건에서 성공하거나 실패한 행동을 나타내는 궤적(trajectory)을 생성한다. 이러한 경험은 강화학습(reinforcement learning), 모방학습(imitation learning), 선호학습(preference learning) 또는 관련 접근법을 통해 결합하여 공통 정책을 개선하는 데 사용할 수 있다. 이후 업데이트된 정책을 검증하고 다시 배포함으로써 플릿 전체에 축적된 경험이 개별 기계의 미래 행동을 개선하도록 할 수 있다.

플릿 지능은 이기종 로봇(heterogeneous robot) 환경에서 특히 가치가 있다. 하나의 플릿에는 서로 다른 적재량, 센서, 매니퓰레이터(manipulator), 이동 시스템, 컴퓨팅 플랫폼, 운용 역할을 가진 기계가 포함될 수 있다. 공유 지능이 모든 로봇이 동일해야 한다는 것을 의미하지는 않는다. 공통 표현(common representation)은 환경 및 작업 지식을 표현하고, 로봇별 모델이나 어댑터(adapter)는 이러한 지식을 각 로봇의 신체 구조(embodiment)와 하드웨어 구성에 적합한 행동으로 변환할 수 있다.

따라서 공유 모델은 명시적인 호환성 관리(compatibility management)를 필요로 한다. 특정 카메라 구성, 가속기(accelerator), 센서 구성 또는 액추에이터 시스템을 대상으로 학습된 모델이 모든 로봇에서 자동으로 유효하다고 가정할 수 없다. 모델 레지스트리(model registry)는 지원되는 하드웨어, 소프트웨어 버전, 보정 요구사항(calibration requirement), 기능, 배포 제약조건을 기록할 수 있다. 플릿 관리는 모든 로봇을 동일한 기계로 취급하지 않고 각 로봇에 적절한 모델 변형(model variant)을 배포할 수 있다.

로봇이 서로 협력하는 환경에서는 버전 일관성(version consistency)이 중요해진다. 서로 다른 기계가 크게 다른 지도, 의미론적 모델, 정책 또는 교통 규칙을 사용하면 상대 로봇의 행동에 대한 예측이 서로 일치하지 않을 수 있다. 플릿 인프라는 활성 모델 버전을 추적하고 단계적 업그레이드(staged upgrade)를 조정할 수 있다. 일부 업데이트는 상호작용하는 로봇 그룹이 함께 전환해야 할 수 있으며, 다른 업데이트는 카나리 배포(canary deployment)를 통해 점진적으로 도입하고 모니터링한 후 더 넓은 범위로 확장할 수 있다.

그러나 공유 모델이 단일 운용 장애점(single point of operational failure)을 만들어서는 안 된다. 중앙 모델 서비스를 사용할 수 없더라도 로봇은 검증된 로컬 모델(known-good local model)과 필수적인 캐시 지식(cached knowledge)을 유지해야 한다. 서버 기반 지능은 의미론적 추론, 최적화, 협업을 향상시킬 수 있지만 즉각적인 안전 행동은 로컬에서 계속 가능해야 한다. 이를 통해 간헐적 연결성(intermittent connectivity) 환경에서 요구되는 로컬 우선 아키텍처(local-first architecture)를 유지할 수 있다.

통신 상태가 저하되면 플릿 지능은 중앙집중형 협업(centralized coordination)에서 부분적 또는 로컬 협업(local cooperation)으로 전환할 수 있다. 로봇은 기존에 할당된 임무를 계속 수행하고, 캐시된 지도를 사용하며, 로컬 교통 규칙을 적용하거나, 지원되는 경우 주변 로봇과 직접 통신할 수 있다. 중앙 연결이 복구되면 연결이 끊어진 동안 의미 있는 활동이 없었다고 가정하는 대신 작업 상태, 지도 변경사항, 관측 정보, 로컬 의사결정을 권한을 가진 플릿 상태(authoritative fleet state)와 다시 조정할 수 있다.

공유 월드 모델(shared world model)은 더욱 심층적인 형태의 플릿 지능을 제공할 수 있다. 현재 로봇 위치와 정적 지도만 유지하는 대신 플릿 수준 월드 모델(fleet-level world model)은 환경이 시간에 따라 어떻게 변화하는지를 표현할 수 있다. 혼잡, 인간 이동, 자원 수요, 경로 가용성(route availability), 로봇 간 상호작용을 예측할 수 있다. 이러한 예측은 운용상의 충돌이 즉각적인 로컬 문제로 발전하기 전에 플릿이 선제적으로 행동하도록 지원할 수 있다.

플릿 수준 추론(fleet-level reasoning)은 개별 로봇이 전역적으로 관리할 수 없는 자원도 최적화할 수 있다. 충전 일정(charging schedule)을 조정하여 너무 많은 로봇이 동시에 운용 불가능한 상태가 되는 것을 방지하고, 작업 부하 균형(workload balancing)을 통해 장비의 마모를 분산하며, 예측 유지보수(predictive maintenance)를 통해 정비가 필요할 가능성이 높은 기계를 식별할 수 있다. 공유 인프라는 도킹 스테이션, 엘리베이터, 문, 매니퓰레이터, 검사 스테이션 등 제한된 자원을 조정하여 각 로봇을 개별적으로 최적화하는 것보다 전체 시스템의 활용률을 높일 수 있다.

클라우드 인프라(cloud infrastructure)는 플릿 지능을 여러 사이트로 확장한다. 서로 다른 시설에서 수집된 경험을 통합하여 글로벌 모델 학습(global model training), 사이트 간 분석(cross-site analytics), 시뮬레이션, 파운데이션 모델 개발(foundation-model development)에 활용할 수 있다. 사이트별 온프레미스 시스템은 로컬 운용 문맥을 유지하고, 클라우드 시스템은 더 큰 로봇 집단에서만 나타나는 패턴을 식별한다. 이후 개선된 글로벌 모델을 각 사이트와 로봇에 맞게 적응(adaptation), 검증(validation), 재배포할 수 있다.

인간 피드백(human feedback) 역시 공유 플릿 지식(shared fleet knowledge)이 될 수 있다. 실제 운용 과정에서 수집된 운영자 수정(operator correction), 유지보수 관측, 시연(demonstration), 선호(preference), 설명을 모델 및 정책 개선에 통합할 수 있다. 한 로봇이 작업을 잘못 이해하여 발생한 수정 사항이 이후 많은 기계에서 유사한 오류가 발생하는 것을 방지할 수 있다. 따라서 플릿은 자율적인 경험뿐 아니라 인간과의 상호작용에서 얻은 구조화된 지식(structured knowledge)도 축적한다.

지능이 공유될수록 보안(security)과 거버넌스(governance)는 더욱 중요해진다. 로봇 신원(robot identity), 접근 권한(access permission), 모델 서명(model signature), 암호화 통신(encrypted communication), 데이터 출처 추적(data provenance), 감사 로그(audit log)를 통해 신뢰할 수 있는 기계와 서비스만 공유 지식에 기여하거나 이를 사용할 수 있도록 해야 한다. 사이트별 또는 민감한 정보는 로컬에 유지하면서 승인된 특징, 통계, 모델 업데이트 또는 익명화된 경험만 더 광범위한 플릿 학습에 참여하도록 할 수 있다.

모니터링(monitoring)은 개별 로봇과 플릿 수준 모두에서 수행되어야 한다. 개별 지표는 로컬 장애를 보여주지만 통합된 통계는 하나의 기계에서는 중요하지 않아 보이는 체계적인 문제를 발견할 수 있다. 플릿 분석(fleet analytics)은 로봇 간 위치 추정 신뢰성, 인식 신뢰도, 인간 개입률(intervention rate), 에너지 소비, 작업 완료, 모델 지연시간, 안전 이벤트, 하드웨어 상태를 비교할 수 있다. 이를 통해 엔지니어링 팀은 개별적인 고장과 공유 모델 또는 시스템 아키텍처 자체의 약점을 구분할 수 있다.

따라서 플릿 지능의 가치는 개별 로봇의 정확도만이 아니라 집단적 결과(collective outcome)를 기준으로 평가해야 한다. 유용한 지표에는 플릿 처리량(fleet throughput), 작업 완료율(task completion rate), 협업 효율성(coordination efficiency), 적응 속도(adaptation speed), 지식 전달 효과(knowledge transfer effectiveness), 통신 오버헤드(communication overhead), 운용 가용성(operational availability), 에너지 효율성, 배포 신뢰성(deployment reliability), 롤백 빈도(rollback frequency), 안전 성능이 포함된다. 개선된 플릿 지능은 개별 로봇을 중앙 서비스에 위험하게 의존시키지 않으면서 전체 플릿의 능력을 향상시켜야 한다.

플릿 지능은 궁극적으로 지속적인 집단 학습 루프(continuous collective learning loop)를 형성한다. 로봇은 안전을 유지할 수 있을 정도로 독립적으로 동작하면서 선택된 경험을 공유 인프라에 제공하고, 다른 기계가 발견한 지식을 전달받으며, 플릿 규모 데이터로 학습된 모델의 이점을 얻는다. 온프레미스 시스템은 로컬 운용을 조정하고 클라우드 시스템은 여러 사이트에 걸친 더 광범위한 학습을 가능하게 한다. 따라서 각 로봇은 동시에 자율 에이전트, 플릿을 위한 센서, 학습 경험의 원천, 집단 지식의 수혜자가 된다.

물리 인공지능의 배포 규모가 확대될수록 공유 모델과 플릿 지능은 자율 시스템의 경제성과 능력을 변화시킨다. 하나의 유용한 경험은 더 이상 그 경험을 획득한 로봇 하나만을 개선하는 데 그치지 않고, 검증을 거친 후 여러 기계를 개선할 수 있다. 한 번 발견된 지도 변화는 전체 사이트를 안내할 수 있으며, 하나의 플랫폼에서 관측된 실패는 다음 세대 모델을 위한 학습 데이터가 될 수 있다. 결과적으로 플릿은 여러 독립적인 로봇의 집합에서 지속적인 실제 세계 운용을 통해 집단적 능력이 향상되는 분산 지능 시스템(distributed intelligence system)으로 발전한다.

## 05.12. Privacy Security and Data Sovereignty

![](images/image12.png){width="7.268055555555556in" height="7.268055555555556in"}

개인정보 보호(privacy), 보안(security), 데이터 주권(data sovereignty)은 로봇이 물리 세계를 지속적으로 관측하고 상호작용하기 때문에 물리 인공지능(Physical AI)의 기본적인 아키텍처 요구사항이다. 카메라, 마이크, 라이다(LiDAR), 위치 시스템(location system), 운용 로그(operational log), 인간과의 상호작용은 사람, 시설, 프로세스, 인프라에 관한 민감한 정보를 포착할 수 있다. 따라서 이러한 보호 기능은 시스템이 이미 배포된 이후 추가하는 것이 아니라 엣지(edge), 온프레미스(on-premise), 클라우드(cloud) 계층 전체에 걸쳐 처음부터 설계되어야 한다.

개인정보 보호는 로봇이 실제로 어떤 정보를 필요로 하는지를 이해하는 것에서 시작한다. 로봇은 안전하게 이동하기 위해 시각 인식(visual perception)이 필요할 수 있지만 관측하는 모든 식별 가능한 이미지를 보존할 필요는 없다. 엣지 처리는 객체, 점유 상태(occupancy), 움직임, 임베딩(embedding), 작업 관련 특징(task-relevant feature)을 추출한 뒤 불필요한 원시 정보를 폐기할 수 있다. 이러한 데이터 최소화 원칙(data-minimization principle)은 개인정보 노출과 전송 또는 저장해야 하는 정보의 양을 동시에 줄인다.

물리 인공지능은 실제 환경에서 센싱(sensing)이 지속적으로 이루어지기 때문에 기존 정보 시스템과 다른 개인정보 보호 문제를 발생시킨다. 모바일 로봇은 의도하지 않게 직원, 고객, 환자, 방문자, 개인 문서, 화면, 대화 또는 제한 구역을 관측할 수 있다. 따라서 센싱 아키텍처(sensing architecture)는 즉각적인 자율성에 필요한 정보와 기록, 학습, 분석 또는 외부 전송이 허용되는 정보를 구분해야 한다.

엣지 지능(edge intelligence)은 첫 번째 개인정보 보호 경계(privacy boundary)를 제공한다. 민감한 원시 센서 데이터는 온보드에 유지하고 파생된 표현(derived representation)이나 승인된 이벤트만 외부 인프라로 전달할 수 있다. 얼굴은 익명화(anonymization)하고, 오디오는 작업 관련 특징으로 변환하며, 고해상도 이미지는 업로드 전에 축소할 수 있다. 정보를 발생원 가까이에서 처리하면 불필요한 노출을 제한하면서도 상위 계산 계층이 유용한 정보를 활용할 수 있다.

온프레미스 인프라(on-premise infrastructure)는 로봇과 전역적으로 연결된 서비스 사이에서 두 번째 보호 경계를 제공한다. 로컬 서버는 플릿 데이터(fleet data)를 통합하고, 개인정보 필터(privacy filter)를 적용하며, 보존 정책(retention policy)을 시행하고, 식별 정보를 제거하며, 접근 권한을 관리하고, 어떤 데이터셋이 시설 외부로 이동할 수 있는지를 결정할 수 있다. 따라서 조직은 모든 운용 데이터를 외부 클라우드 환경으로 자동 전송하지 않고도 상당한 규모의 공유 컴퓨팅 자원을 사용할 수 있다.

클라우드 인프라(cloud infrastructure)는 보다 광범위한 처리에 적합한 정보만 수신해야 한다. 대규모 학습과 플릿 분석(fleet analytics)은 여러 사이트에서 수집한 데이터의 이점을 얻을 수 있지만, 이것이 제한 없는 원시 센서 스트림을 중앙집중화해야 한다는 의미는 아니다. 큐레이션된 데이터셋(curated dataset), 익명화된 샘플, 통계적 요약, 임베딩 또는 승인된 학습 기록을 사용하면 사이트별 정보나 개인 식별 정보(personally identifiable information)의 노출을 줄이면서 학습 가치의 상당 부분을 유지할 수 있다.

데이터 주권(data sovereignty)은 누가 데이터를 통제하는지, 어디에 저장할 수 있는지, 어디에서 처리할 수 있는지, 그리고 어떤 법적 또는 조직적 권한 아래에서 이동할 수 있는지를 결정함으로써 개인정보 보호의 개념을 확장한다. 데이터셋을 기술적으로 클라우드에 전송할 수 있더라도 시설, 기업, 지역 또는 관할권(jurisdiction) 외부로 이동하는 것이 금지될 수 있다. 따라서 작업 배치(workload placement)는 지연시간, 연산 능력, 대역폭, 에너지, 비용뿐 아니라 거버넌스 제약(governance constraint)도 함께 고려해야 한다.

데이터 주권 요구사항은 엣지-클라우드 분할(edge-cloud partition)을 직접적으로 변화시킬 수 있다. 민감한 센서 스트림은 로봇에 유지하고, 조직별 운용 데이터는 온프레미스에 유지하며, 승인된 정보만 글로벌 인프라로 이동시킬 수 있다. 일부 배포 환경에서는 원천 데이터를 사이트 외부로 이동할 수 없기 때문에 모델 학습 자체도 로컬에서 수행해야 할 수 있다. 따라서 계층형 컴퓨팅(hierarchical compute)은 계산 효율성을 향상시키는 수단일 뿐 아니라 정보 경계(information boundary)를 강제하는 메커니즘이 된다.

보안(security)은 이러한 분산 아키텍처를 승인되지 않은 접근, 조작, 사칭(impersonation), 악의적인 업데이트로부터 보호한다. 물리 인공지능 시스템은 센서, 액추에이터, 임베디드 제어기(embedded controller), 엣지 컴퓨터, 로컬 서버, 클라우드 서비스, 운영자 인터페이스(operator interface), 다른 로봇을 연결한다. 모든 연결은 잠재적인 신뢰 경계(trust boundary)를 형성한다. 따라서 전체 시스템에 걸쳐 신원, 통신, 소프트웨어, 모델, 명령, 저장 데이터, 배포 프로세스를 보호해야 한다.

플릿 환경에서는 로봇 신원(robot identity)이 필수적이다. 인프라는 모델을 요청하거나, 데이터를 업로드하거나, 지도 변경을 보고하거나, 작업을 수락하는 장치가 승인된 로봇인지를 판단할 수 있어야 한다. 강력한 장치 신원(device identity)과 인증(authentication) 메커니즘은 알 수 없는 시스템이 신뢰할 수 있는 플릿 구성원으로 참여하는 것을 방지한다. 동일한 신원 제어는 서버, 운영자 애플리케이션, 유지보수 도구, 자동화 서비스에도 적용되어야 한다.

인증(authentication)은 개체(entity)가 실제로 신뢰할 수 있는 대상인지를 확인하고, 권한 부여(authorization)는 인증된 개체가 무엇을 수행할 수 있는지를 결정한다. 유지보수 기술자는 진단 정보를 검사할 수 있지만 내비게이션 정책을 변경할 권한은 없을 수 있다. 로봇은 운용 데이터를 업로드할 수 있지만 다른 로봇의 설정을 변경할 수 없어야 한다. 역할 기반 및 정책 기반 접근 제어(role- and policy-based access control)는 자격 증명(credentials)이 손상되거나 부적절한 권한이 사용될 때 발생할 수 있는 피해를 줄인다.

로봇, 온프레미스 인프라, 클라우드 서비스 사이의 통신은 도청(interception)과 변조(modification)로부터 보호되어야 한다. 암호화(encryption)는 기밀성(confidentiality)을 보호하고 무결성 메커니즘(integrity mechanism)은 변경된 메시지를 탐지하는 데 도움을 준다. 임무 명령, 지도, 자격 증명, 모델 패키지, 안전 설정, 플릿 협업 정보는 디지털 정보의 조작이 궁극적으로 물리 세계의 결과로 이어질 수 있기 때문에 보안 채널(secure channel)이 특히 중요하다.

저장된 정보도 이에 상응하는 보호가 필요하다. 로봇 로그, 지도, 기록된 센서 데이터, 모델 결과물(model artifact), 보정 정보(calibration information), 자격 증명, 캐시된 임무 정보는 로봇이 오프라인 상태에서도 공격자에게 가치가 있을 수 있다. 저장 데이터 암호화(encryption at rest), 보호된 키 저장소(protected key storage), 통제된 접근, 안전한 삭제(secure deletion)를 적용하면 장치, 디스크 또는 서버가 분실되거나 도난당하거나 정비 과정에서 정상적인 운용 범위를 벗어나 접근될 때의 정보 노출을 줄일 수 있다.

로봇은 기존 데이터센터 외부에서 운용되므로 물리적 접근(physical access) 역시 고려해야 한다. 공격자나 승인되지 않은 사람이 통신 포트, 저장장치, 디버그 인터페이스(debug interface), 이동식 미디어(removable media), 온보드 컴퓨터에 직접 접근할 수 있다. 보안 부팅(secure boot), 하드웨어 기반 키(hardware-backed key), 제한된 디버깅, 보호된 설정, 변조 인지 설계(tamper-aware design)를 적용하면 물리적 접근이 로봇의 소프트웨어와 지능에 대한 무제한 제어로 이어질 가능성을 낮출 수 있다.

인공지능 모델이 인식, 예측, 계획, 의사결정에 직접적인 영향을 주기 때문에 모델 보안(model security)은 특히 중요하다. 변조된 모델은 대부분의 상황에서 정상적으로 동작하면서 특정 조건에서만 실패할 수도 있다. 따라서 모델은 일반 파일이 아니라 통제되는 실행 지능(controlled executable intelligence)으로 취급해야 한다. 실제 운용 로봇에서 활성화하기 전에 버전 관리(version management), 암호학적 서명(cryptographic signing), 무결성 검증(integrity verification), 호환성 검사(compatibility check), 배포 승인(deployment approval)을 수행해야 한다.

보안 부팅과 신뢰할 수 있는 소프트웨어 체인(trusted software chain)은 이러한 원칙을 모델에서 전체 런타임 환경으로 확장할 수 있다. 각각의 소프트웨어 계층은 실행 전에 다음 구성요소의 무결성을 검증하여 펌웨어(firmware), 운영체제, 미들웨어(middleware), 추론 엔진(inference engine), 인공지능 모델까지 이어지는 신뢰 체계를 구축할 수 있다. 목표는 최종적으로 물리적 행동을 생성하는 체인에 승인되지 않은 코드가 은밀하게 진입하는 것을 방지하는 것이다.

모델 다운로드(model download)는 하향 정보 경로가 로봇 행동을 직접 변경할 수 있으므로 강력한 보호가 필요하다. 로봇은 업데이트를 활성화하기 전에 출처(origin), 서명(signature), 버전, 하드웨어 호환성, 무결성을 검증해야 한다. 새로운 모델은 비활성 저장공간(inactive storage)에 배치하고 현재 검증된 버전은 계속 운용할 수 있다. 검증에 실패하면 정상 동작이 확인된 구성(known-good configuration)을 손상시키는 대신 업데이트를 거부해야 한다.

데이터 업로드(data upload)는 반대 방향에서 동일한 수준의 신뢰 제어를 필요로 한다. 학습 인프라는 어떤 로봇이 관측을 생성했는지, 당시 어떤 소프트웨어 및 모델 버전이 활성화되어 있었는지, 전송 중 기록의 무결성이 유지되었는지를 확인할 수 있어야 한다. 출처 추적(provenance)이 없으면 손상되거나 악의적인 데이터가 데이터셋을 오염시키고 미래 모델에 영향을 줄 수 있다. 따라서 신뢰할 수 있는 데이터 계보(trusted data lineage)는 사이버보안과 물리 인공지능 학습 품질을 연결한다.

플릿 지능(fleet intelligence)은 공유 정보의 가치와 위험을 동시에 증가시킨다. 하나의 로봇이 침해되었다고 해서 전체 플릿의 지도, 정책, 모델 또는 데이터에 자동으로 무제한 접근할 수 있어서는 안 된다. 세분화(segmentation)와 최소 권한 접근(least-privilege access)을 이용하면 각 로봇과 서비스가 접근할 수 있는 범위를 제한할 수 있다. 공유 지능은 하나의 구성요소 침해가 즉시 모든 연결된 기계의 침해로 확산되지 않도록 설계되어야 한다.

네트워크 세분화(network segmentation)는 안전 중요 제어(safety-critical control), 로봇 운용, 관리 서비스, 학습 인프라, 외부 클라우드 통신을 서로 분리할 수 있다. 분석 서비스의 취약점이 액추에이터 제어로 직접 연결되는 경로를 제공해서는 안 된다. 게이트웨이(gateway)는 영역 사이의 트래픽을 중재하고, 정책을 적용하며, 요청을 검사하고, 허용되는 통신 경로를 제한할 수 있다. 따라서 아키텍처적 분리는 개별 소프트웨어 장애나 보안 침해가 발생했을 때 그 영향을 줄인다.

오프라인 자율성(offline autonomy) 역시 보안 회복탄력성(security resilience)에 기여한다. 원격 서비스에 지속적으로 의존하는 로봇은 해당 서비스가 의도적으로 방해받으면 운용할 수 없게 될 수 있다. 필수적인 인식, 위치 추정, 계획, 안전 기능은 로컬에 유지하여 네트워크 접근 차단이 기본적인 자율성을 자동으로 제거하지 않도록 해야 한다. 연결 손실이 발생하면 선택적 지능을 비활성화하면서 로봇은 통제된 로컬 운용 모드(controlled local operating mode)로 전환할 수 있다.

개인정보 보호 정책(privacy policy)은 데이터 수집뿐 아니라 보존(retention)도 포함해야 한다. 진단이나 학습을 위해 정당하게 기록된 정보라고 해서 무기한 저장할 필요는 없다. 서로 다른 데이터 유형에는 운용, 엔지니어링, 계약 또는 규제 요구사항에 따라 서로 다른 보존 기간(retention period)을 적용할 수 있다. 자동 만료(automated expiration)와 삭제는 지속적인 가치가 있는 기록을 보존하면서 장기적인 정보 노출을 줄인다.

목적 제한(purpose limitation)은 또 다른 중요한 거버넌스 원칙이다. 내비게이션 안전을 위해 수집한 데이터가 이미 존재한다는 이유만으로 관련 없는 분석에 자동으로 사용되어서는 안 된다. 메타데이터(metadata)는 데이터셋에 허용된 목적, 민감도(sensitivity), 소유권, 보존 기간, 처리 위치를 기술할 수 있다. 그러면 인프라는 모든 로봇 데이터를 동일하게 취급하는 대신 정보의 의미와 거버넌스 상태에 따라 정책을 적용할 수 있다.

감사 가능성(auditability)은 민감한 정보와 운용 지능이 시스템을 통해 어떻게 이동하는지를 이해하는 데 필요하다. 로그는 누가 데이터에 접근했는지, 어떤 로봇이 데이터를 업로드했는지, 어디에서 처리되었는지, 어떤 모델이 이를 사용했는지, 이후 어떤 모델 버전이 배포되었는지를 기록할 수 있다. 이러한 추적 가능성(traceability)은 보안 조사, 규정 준수(compliance), 디버깅, 모델 계보(model lineage)를 지원하고 거버넌스 정책이 실제로 적용되고 있다는 증거를 제공한다.

개인정보 보호형 학습(privacy-preserving learning)은 원시 경험을 중앙집중화할 필요성을 줄일 수 있다. 적절한 경우 각 사이트는 모델을 로컬에서 학습하거나 적응시키고 전체 센서 데이터셋 대신 모델 업데이트, 선택된 표현, 통합된 정보를 교환할 수 있다. 구체적인 접근법은 학습 작업과 위협 모델(threat model)에 따라 달라지지만 아키텍처 원칙은 동일하다. 유용한 집단 학습(collective learning)을 위해 모든 물리 세계 관측을 제한 없이 이동시킬 필요는 없다.

보안 모니터링(security monitoring)은 로봇과 인프라 계층 전체에서 지속적으로 동작해야 한다. 인증 실패, 비정상적인 데이터 전송, 예상하지 못한 모델 변경, 비정상적인 명령, 반복적인 연결 시도, 설정 변경, 비정상적인 로봇 행동은 침해(compromise)의 징후가 될 수 있다. 사이버보안 이벤트와 물리적 행동의 상관관계를 분석하는 것은 특히 중요하다. 물리 인공지능에 대한 공격이 비정상적인 움직임, 인식 실패 또는 운용 편차(operational deviation)의 형태로 처음 나타날 수 있기 때문이다.

사고 복구(incident recovery)는 침해가 발생하기 전에 설계되어야 한다. 시스템은 자격 증명 폐기(credential revocation), 로봇 격리(robot isolation), 모델 롤백(model rollback), 키 교체(key rotation), 소프트웨어 복구, 신뢰할 수 있는 구성의 복원을 지원해야 한다. 의심스러운 로봇을 전체 플릿을 오프라인으로 전환하지 않고 공유 플릿 서비스에서 분리할 수 있어야 한다. 복구 메커니즘은 보안 제어를 통해 문제를 격리하면서 가능한 한 많은 안전한 운용 능력을 유지하도록 한다.

개인정보 보호, 보안, 데이터 주권은 궁극적으로 모델 배치(model placement), 작업 부하 분할(workload partitioning), 데이터 전송, 플릿 협업, 생애주기 관리(lifecycle management)에 함께 영향을 주어야 한다. 계산 관점에서 가장 빠르거나 가장 저렴한 아키텍처라 하더라도 민감한 데이터를 노출하거나 정보 통제 요구사항을 위반한다면 적절하지 않을 수 있다. 올바른 아키텍처는 물리 인공지능의 전체 생애주기에서 지능, 운용 성능, 보안 경계, 조직 정책, 정보 통제 사이의 균형을 유지해야 한다.

따라서 신뢰할 수 있는 물리 인공지능 아키텍처(trustworthy Physical AI architecture)는 로컬 우선(local-first) 및 통제된 공유(controlled-sharing) 원칙을 따른다. 민감한 정보는 가능한 한 발생원 가까이에 유지하고, 상위 계층에는 처리 권한이 부여된 정보만 전달하며, 모든 참여자를 인증하고, 데이터와 모델을 전송 및 저장 과정에서 보호하며, 모델 배포는 검증 가능한 신뢰 체인(verifiable trust chain)을 따라야 한다. 계층형 컴퓨팅은 단순한 성능 아키텍처가 아니라 개인정보 보호 및 보안 아키텍처가 된다.

물리 인공지능이 개별 로봇에서 플릿과 여러 사이트로 확장될수록 데이터는 단순한 센싱의 부산물이 아니라 전략적인 운용 자산(strategic operational asset)이 된다. 조직은 로봇이 무엇을 관측하는지, 어떤 정보를 보존하는지, 데이터가 어디로 이동하는지, 누가 접근할 수 있는지, 데이터가 학습에 어떻게 기여하는지, 그리고 어떤 지능이 다시 로봇으로 전달되는지를 파악해야 한다. 개인정보 보호, 보안, 데이터 주권은 분산된 물리 인공지능이 그 지능의 기반이 되는 물리 세계 정보에 대한 통제권을 유지하면서 학습하고 협업할 수 있도록 하는 거버넌스 프레임워크(governance framework)를 제공한다.

## 05.13. Edge Cloud Partitioning Analysis [w/Code]

![](images/image13.png){width="7.268055555555556in" height="7.268055555555556in"}

엣지-클라우드 분할 분석(edge-cloud partitioning analysis)은 물리 인공지능(Physical AI)의 작업 부하(workload)를 온보드 엣지 컴퓨팅(onboard edge computing), 인접한 온프레미스 인프라(on-premise infrastructure), 원격 클라우드 자원(cloud resource) 사이에 어떻게 분산할지를 결정한다. 이러한 분석은 계산 성능만이 아니라 센싱에서 행동까지 이어지는 전체 시스템(sensing-to-action system)을 고려해야 한다. 지연시간(latency), 안전성(safety), 연결성(connectivity), 대역폭(bandwidth), 에너지(energy), 메모리(memory), 개인정보 보호(privacy), 작업 규모(workload scale), 장애 시 동작(failure behavior)이 각 기능의 실행 위치를 종합적으로 결정한다.

첫 번째 분석 단계는 물리 인공지능 파이프라인(Physical AI pipeline)의 주요 작업 부하를 모두 식별하는 것이다. 대표적인 작업에는 센서 전처리(sensor preprocessing), 인식(perception), 위치 추정(localization), 센서 융합(sensor fusion), 월드 모델링(world modeling), 예측(prediction), 계획(planning), 제어(control), 의미론적 추론(semantic reasoning), 플릿 협업(fleet coordination), 데이터 처리, 시뮬레이션(simulation), 학습(training), 분석(analytics)이 포함된다. 각각의 작업은 서로 다른 시간 및 자원 요구사항을 가지므로 전체 인공지능 스택(AI stack)을 하나의 분할 불가능한 계산 작업으로 취급하면 비효율적인 배치 결정으로 이어질 수 있다.

지연시간(latency)은 가장 명확한 분할 기준 중 하나를 제공한다. 빠른 센싱-행동 루프(sensing-to-action loop)에 직접 참여하는 작업은 네트워크 통신으로 인해 지연과 지터(jitter)가 발생하기 때문에 일반적으로 엣지에서 실행해야 한다. 장애물 탐지, 비상 대응, 로컬 궤적 생성(local trajectory generation), 상태 추정(state estimation), 액추에이터 관련 의사결정은 밀리초 단위의 결과를 요구할 수 있다. 통신 지연으로 인해 결과가 로봇에 도착하기 전에 이미 유효하지 않게 될 수 있다면 원격 실행은 적합하지 않다.

안전 중요도(safety criticality)는 또 다른 배치 제약조건으로 표현할 수 있다. 기능의 실패가 즉각적으로 위험한 물리적 행동을 발생시킬 수 있다면 일반적으로 로컬 구현(local implementation) 또는 폴백(fallback)을 갖추어야 한다. 클라우드 자원이 더 뛰어난 추론이나 추가적인 예측을 제공할 수 있더라도 로봇이 물리적으로 안전한 상태를 유지하기 위해 원격 응답을 반드시 필요로 해서는 안 된다. 따라서 분할 분석에서는 선택적인 지능 향상(optional intelligence enhancement)과 안전한 운용에 필요한 최소 자율 기능(minimum autonomous capability)을 구분해야 한다.

연산 집약도(computational intensity)는 작업을 상위 인프라 계층으로 이동시키는 요인이 된다. 대규모 월드 모델(large world model), 멀티모달 파운데이션 모델(multimodal foundation model), 광범위한 탐색(search), 전역 최적화(global optimization), 시뮬레이션, 학습은 모바일 하드웨어가 효율적으로 제공할 수 있는 수준을 훨씬 넘어서는 메모리와 가속기 용량을 요구할 수 있다. 온프레미스 GPU 서버는 비교적 낮은 네트워크 지연시간으로 중간 규모 작업을 처리하고, 클라우드 인프라는 규모가 크지만 즉각적인 시간 제약이 없는 계산에 탄력적인 자원(elastic resource)을 제공할 수 있다.

실용적인 분석에서는 각각의 작업 부하를 다차원 요구사항 벡터(multidimensional requirement vector)로 표현할 수 있다. 관련 변수에는 최대 허용 지연시간, 실행 빈도(execution frequency), 연산 요구량, 가속기 메모리, 입력 크기, 출력 크기, 전력 소비, 열 부하(thermal load), 안전 중요도, 개인정보 민감도(privacy sensitivity), 네트워크 허용도(network tolerance), 요구 가용성(required availability)이 포함될 수 있다. 이러한 요구사항을 각 계산 계층의 능력과 비교하면 직관에만 의존하지 않고 체계적인 배치 기준을 구축할 수 있다.

엣지 능력(edge capability) 역시 측정 가능한 제약조건으로 표현할 수 있다. 사용 가능한 CPU, GPU, NPU, 메모리, 저장공간, 전력 예산(power budget), 냉각 능력(cooling capacity), 센서 대역폭, 예상 임무 수행시간(expected mission runtime)이 실제 온보드 컴퓨팅 범위(computing envelope)를 결정한다. 작업이 기술적으로 로봇에서 실행될 수 있더라도 지속적인 실행으로 열 스로틀링(thermal throttling)이 발생하거나 배터리 운용시간이 과도하게 줄어들거나 우선순위가 높은 인식 및 안전 기능이 마감시간을 충족하지 못한다면 적합하지 않을 수 있다.

온프레미스 인프라는 물리적 운용 위치와의 근접성을 유지하면서 공유 계산 능력(shared computational capacity)을 추가한다. 분석에는 로컬 네트워크 지연시간, 서버 가속기 용량, 동시 실행성(concurrency), 플릿 규모(fleet size), 저장공간, 가용성, 이중화(redundancy)를 포함해야 한다. 하나의 대규모 모델을 빠르게 실행할 수 있는 서버라도 수십 대의 로봇이 동시에 추론을 요청하면 병목(bottleneck)이 될 수 있다. 따라서 분할에서는 단일 요청의 벤치마크 성능이 아니라 시스템 수준 수요(system-level demand)를 평가해야 한다.

클라우드 분석(cloud analysis)은 다른 종류의 가정을 필요로 한다. 계산 능력은 매우 높은 수준으로 확장할 수 있지만 통신 경로는 더 길고 결정성(determinism)이 낮다. 광역 네트워크 지연시간, 대역폭 비용, 서비스 가용성, 데이터 전송량, 보안, 데이터 주권(data sovereignty)이 중요한 요소가 된다. 따라서 클라우드 실행은 대규모 학습, 시뮬레이션, 글로벌 분석(global analytics), 사이트 간 모델 개선(cross-site model improvement)처럼 즉각적인 응답보다 계산 규모와 공유 학습(shared learning)의 가치가 더 큰 작업에 적합하다.

연산 오프로딩(computation offloading)은 비용을 제거하는 것이 아니라 로컬 계산 비용을 데이터 전송과 원격 실행 비용으로 변환하기 때문에 통신 비용(communication cost)을 명시적으로 포함해야 한다. 원시 멀티카메라 영상이나 고밀도 라이다 스트림을 전송하면 막대한 대역폭이 필요할 수 있다. 대신 엣지 전처리를 통해 객체, 특징(feature), 점유 표현(occupancy representation), 압축된 관측, 이벤트 요약을 생성할 수 있다. 따라서 최적의 분할은 모델의 계산량 자체보다 중간 표현(intermediate representation)의 크기에 더 크게 좌우될 수도 있다.

분석 모델은 데이터 준비(data preparation), 업링크 전송(uplink transmission), 네트워크 전파(network propagation), 대기열(queueing), 원격 추론(remote inference), 다운링크 전송(downlink transmission), 로컬 통합(local integration)의 합으로 종단 간 원격 실행시간(end-to-end remote execution time)을 추정할 수 있다. 물리 인공지능은 최악의 경우 또는 높은 백분위 지연 특성(high-percentile behavior)에 의존하는 경우가 많으므로 평균 지연시간만으로는 충분하지 않다. 지연된 결과가 계획이나 물리적 상호작용에 영향을 줄 수 있다면 변동성과 지터도 함께 고려해야 한다.

에너지 분석(energy analysis)은 지연시간 분석만으로 발견하기 어려운 절충관계(tradeoff)를 보여줄 수 있다. 모델을 온보드에서 실행하면 가속기 전력을 소비하고 열을 발생시키지만 데이터를 전송하는 것 역시 통신 에너지를 소비한다. 오프로딩은 절감되는 계산 및 열 부담이 통신과 외부 의존성 비용을 정당화할 때만 유리하다. 대규모 원시 센서 스트림의 경우 강력한 원격 컴퓨팅을 사용할 수 있더라도 로컬 특징 추출(local feature extraction)이 지속적인 데이터 전송보다 전체 에너지를 적게 소비할 수 있다.

메모리 분석(memory analysis) 역시 중요하다. 모델이 계산 처리량 요구사항을 만족하더라도 모델 가중치(model weight), 활성화 텐서(activation tensor), 시간 버퍼(temporal buffer), 지도, 캐시(cache), 동시 실행 애플리케이션을 포함하면 실제 메모리 한계를 초과할 수 있다. 분할 분석에서는 전체 로봇 작업 부하에 걸친 최대 메모리 사용량(peak memory usage)과 대역폭 경합(bandwidth contention)을 평가해야 한다. 하나의 메모리 집약적 기능을 로봇 외부로 이동시키는 것이 다른 부분의 연산량을 줄이는 것보다 더 큰 효과를 제공할 수도 있다.

연결성(connectivity)은 이진적인 전제가 아니라 확률 또는 운용 상태 변수(operating-state variable)로 표현할 수 있다. 강한 연결(strong connectivity), 저하된 연결(degraded connectivity), 간헐적 연결(intermittent connectivity), 오프라인(offline) 상태에서는 각각 가능한 분할 방식이 달라진다. 연결이 강하면 고부하 추론을 오프로딩할 수 있고, 연결이 저하되면 원격 의존성을 줄일 수 있다. 오프라인 운용에서는 필수 작업을 로컬에 유지해야 한다. 따라서 강건한 분할은 이상적인 네트워크 환경뿐 아니라 여러 연결 상태에서 평가되어야 한다.

분석에서는 최적화를 시작하기 전에 하드 제약조건(hard constraint)을 정의할 수 있다. 안전 중요 제어는 온보드에 유지해야 하고, 민감한 데이터는 사이트 외부로 이동할 수 없으며, 일부 작업에는 엄격한 지연시간 제한이 적용될 수 있다. 이러한 제약조건을 위반하는 후보 배치(candidate placement)는 제거한다. 이후 남아 있는 구성에 대해 에너지, 인프라 비용, 처리량(throughput), 모델 품질, 네트워크 사용량, 플릿 효율성 등의 소프트 목표(soft objective)를 비교할 수 있다.

가중 점수 모델(weighted scoring model)은 후보 배치를 비교하는 간단한 방법을 제공한다. 각각의 작업에 지연시간 민감도, 안전 중요도, 연산 요구량, 대역폭 요구량, 개인정보 민감도, 연결 허용도, 공유 이점(sharing benefit)에 대한 점수를 부여할 수 있다. 이후 엣지, 온프레미스, 클라우드 위치를 이러한 특성에 대해 평가할 수 있다. 가중치(weighting)를 조정하면 임무별 요구사항을 반영할 수 있는데, 예를 들어 재난 대응 로봇과 창고 자율이동로봇(AMR)은 동일한 요소에 서로 다른 중요도를 부여할 수 있다.

더 발전된 방식에서는 분할을 제약 최적화 문제(constrained optimization problem)로 취급할 수 있다. 결정 변수(decision variable)는 작업이 어디에서 실행되는지를 나타내며, 목적 함수(objective function)는 지연시간, 에너지, 네트워크 트래픽, 운용 비용 또는 위험의 조합을 최소화하도록 정의할 수 있다. 제약조건에는 가속기 용량, 메모리, 전력, 대역폭, 마감시간 요구사항, 개인정보 규칙, 서비스 가용성이 포함될 수 있다. 이러한 모델은 많은 작업과 로봇이 공유 자원을 놓고 경쟁하는 환경에서 특히 유용하다.

분할에서는 작업 간 의존성(dependency)도 고려해야 한다. 인식을 서버로 이동하면 계획에 필요한 데이터가 달라지며, 월드 모델을 이동하면 상태 이력(state history)이나 중간 표현을 전송해야 할 수 있다. 독립적으로 보면 원격 실행에 적합한 두 작업이라도 서로 분리할 경우 과도한 통신이 발생할 수 있다. 그래프 기반 분석(graph-based analysis)은 작업을 노드(node), 데이터 의존성을 엣지(edge)로 표현하여 계산량과 통신량을 함께 고려하면서 분할 경계를 평가할 수 있다.

최적 경계는 완전한 애플리케이션 사이가 아니라 모델 내부에 존재할 수도 있다. 분할 추론(split inference)은 신경망의 초기 계층을 로봇에서 실행하고 이후 계층을 서버에서 실행할 수 있다. 중간 특징이 원시 센서 데이터보다 작고 나머지 모델의 계산 비용이 높은 경우 유용할 수 있다. 그러나 신경망 내부의 분할은 동기화(synchronization), 호환성, 특징 전송(feature transfer), 장애 복구 요구사항을 추가하므로 이러한 요소도 분석에 포함해야 한다.

작업 빈도(workload frequency)는 배치의 경제성을 변화시킨다. 30Hz로 실행되는 중간 규모 모델은 온보드 자원의 대부분을 소비할 수 있지만 수 분에 한 번 호출되는 훨씬 큰 모델은 운용에 미치는 영향이 작을 수 있다. 따라서 분석에서는 단일 추론 비용뿐 아니라 지속적인 계산 요구량(sustained compute demand)을 사용해야 한다. 불확실하거나 새로운 상황, 어려운 상황에서만 고비용 추론이 필요한 경우 이벤트 기반 오프로딩(event-triggered offloading)이 특히 효과적일 수 있다.

플릿 규모(fleet scale)가 증가하면 분할 결정도 달라진다. 로컬 서버가 5대의 로봇에는 우수한 성능을 제공하지만 50대에서는 과부하될 수 있다. 공유 모델 서빙(shared model serving)에서는 요청 도착률(request arrival rate), 배칭(batching), 대기열, 가속기 활용률(accelerator utilization), 장애 집중(failure concentration)을 분석해야 한다. 클라우드 자원은 변동하는 수요를 흡수할 수 있고 여러 온프레미스 서버는 예측 가능한 로컬 용량을 제공할 수 있다. 따라서 초기 배포 규모뿐 아니라 예상되는 플릿 성장까지 고려해야 한다.

개인정보 보호와 데이터 주권은 하드 아키텍처 제약조건(hard architectural constraint)으로 작용할 수 있다. 원시 이미지, 오디오, 시설 지도 또는 독점적인 운용 정보는 온보드 또는 로컬 인프라 내부에 유지해야 할 수 있다. 이러한 경우 원격 처리를 위해서는 전송 전에 익명화(anonymization) 또는 특징 추출을 수행해야 할 수 있다. 데이터 소유권, 보존, 처리 위치 또는 관할권 요구사항을 위반한다면 계산 관점에서 매력적인 클라우드 분할이라도 유효한 선택이 아니다.

신뢰성 분석(reliability analysis)은 각각의 계산 계층에 장애가 발생했을 때 어떤 일이 일어나는지를 평가한다. 엣지 장애는 자율 운용을 직접 중단시킬 수 있지만 온프레미스 또는 클라우드 서비스의 손실은 최소한의 안전 기능을 제거하지 않고 기능만 저하시키는 것이 이상적이다. 후보 분할은 서버 장애, 패킷 손실, 과도한 지연시간, 대역폭 붕괴(bandwidth collapse), 완전한 연결 단절 등의 장애 시나리오를 통해 평가할 수 있다. 선호되는 아키텍처는 갑작스러운 자율성 상실이 아니라 예측 가능한 점진적 성능 저하(graceful degradation)를 보여야 한다.

런타임 적응(runtime adaptation)은 하나의 정적인 분할(static partition)보다 더 나은 결과를 제공할 수 있다. 시스템은 네트워크 품질, GPU 활용률, 온도, 배터리 상태, 작업 수요, 불확실성(uncertainty), 서버 가용성을 지속적으로 모니터링할 수 있다. 연결 상태가 좋지 않을 때는 작업을 로컬에 유지하고 외부 자원을 신뢰성 있게 사용할 수 있을 때 작업을 이동하거나 상위 계층으로 에스컬레이션(escalation)할 수 있다. 따라서 동적 분할(dynamic partitioning)은 작업 배치를 설계 단계의 결정에서 운용 중 자원 관리 정책(resource-management policy)으로 확장한다.

검증(validation)은 실제 대표 하드웨어와 네트워크에서 후보 분할을 테스트해야 한다. 분석적 추정값은 측정된 종단 간 지연시간, 전력 소비, 메모리 사용량, 열 특성, 대역폭, 대기열 지연(queueing delay), 임무 성능과 비교해야 한다. 네트워크 장애 테스트(network impairment testing)를 통해 혼잡과 연결 단절을 재현하고, 플릿 규모 부하 테스트(fleet-scale load testing)를 통해 서버 병목을 발견할 수 있다. 측정된 동작이 이론적인 가정과 다르면 분할 결정을 수정해야 한다.

엣지-클라우드 분할 분석의 최종 결과가 반드시 하나의 보편적인 배치 맵(universal placement map)일 필요는 없다. 서로 다른 운용 모드에는 서로 다른 구성이 필요할 수 있다. 고성능 모드(high-performance mode)는 광범위한 온프레미스 추론을 활용하고, 에너지 절약 모드(energy-saving mode)는 일부 작업을 오프로딩하며, 오프라인 모드(offline mode)는 거의 모든 기능을 로컬 지능에 의존할 수 있다. 따라서 아키텍처는 여러 개의 검증된 분할 구성(validated partition)을 정의하고 임무 및 인프라 조건에 따라 이들 사이를 전환할 수 있다.

성공적인 분석은 물리 인공지능의 기본적인 계층 구조를 유지한다. 즉각적이고 안전 중요도가 높은 지능은 물리 시스템 가까이에 유지하고, 공유 운용 지능(shared operational intelligence)은 인접한 인프라를 사용하며, 대규모 계산 또는 긴 시간 범위(long time horizon)를 요구하는 작업은 클라우드 방향으로 이동한다. 정확한 경계는 엣지 또는 클라우드 컴퓨팅 중 어느 하나가 본질적으로 우수하다는 고정된 가정이 아니라 작업 특성과 시스템 제약조건을 정량적으로 분석하여 결정해야 한다.

궁극적으로 엣지-클라우드 분할 분석은 아키텍처의 절충관계(tradeoff)를 측정 가능한 엔지니어링 의사결정으로 변환한다. 지연시간, 안전성, 연산 능력, 메모리, 에너지, 열 한계, 대역폭, 연결성, 개인정보 보호, 신뢰성, 플릿 규모를 함께 평가함으로써 설계자는 지능이 어디에서 실행되어야 하는지, 그리고 조건 변화에 따라 그 경계를 어떻게 변경해야 하는지를 결정할 수 있다. 그 결과 실시간 로컬 자율성(real-time local autonomy)과 확장 가능한 외부 지능(scalable external intelligence)을 결합하면서 예측 가능하고 효율적이며 회복탄력적인 물리적 운용을 유지하는 계층형 물리 인공지능 아키텍처(hierarchical Physical AI architecture)를 구축할 수 있다.
