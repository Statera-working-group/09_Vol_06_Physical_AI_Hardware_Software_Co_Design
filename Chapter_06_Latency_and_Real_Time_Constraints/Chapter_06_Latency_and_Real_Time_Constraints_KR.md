**Volume 06. Physical AI Hardware Software Co Design**

# Chapter 06. Latency and Real Time Constraints

## 06.01. Real Time Requirements in Physical AI

![](images/image1.png){width="7.268055555555556in" height="7.268055555555556in"}

물리 인공지능(Physical AI)은 지능이 지속적으로 변화하는 물리적 환경(physical environment)과 직접 결합되어 있기 때문에 실시간 요구사항(real-time requirements) 아래에서 동작한다. 많은 디지털 인공지능(digital AI) 응용과 달리, 로봇은 계산이 끝날 때까지 단순히 기다렸다가 반응할 수 없다. 추론(inference)이 실행되는 동안에도 센서는 계속 데이터를 생성하고, 물체는 계속 움직이며, 로봇 자체의 위치도 변한다. 따라서 실시간 성능(real-time performance)이란 단순히 빠르게 계산하는 것이 아니라, 결정이 물리적으로 유효한 시간 범위 안에서 충분히 정확한 판단을 생성하는 것을 의미한다.

실시간(real time)의 의미는 하나의 보편적인 지연시간 목표(latency target)로 결정되는 것이 아니라 구현된 시스템(embodied system)의 동역학(dynamics)에 따라 달라진다. 천천히 움직이는 창고 로봇(warehouse robot)은 수십 또는 수백 밀리초의 인지(perception)나 계획(planning) 지연을 허용할 수 있지만, 고속 차량, 드론(drone), 매니퓰레이터(manipulator), 동적 균형 로봇(dynamically balancing robot)은 불과 몇 밀리초 이내의 중요한 반응을 요구할 수 있다. 따라서 타이밍 요구사항(timing requirements)은 이동 속도, 액추에이터 동역학(actuator dynamics), 환경 불확실성(environmental uncertainty), 안전 여유(safety margin), 작업 복잡도(task complexity)를 기반으로 결정해야 한다.

물리 인공지능 시스템(Physical AI system)은 일반적으로 서로 다른 주파수(frequency)로 동작하는 여러 계산 루프(computational loops)를 포함한다. 모터 전류 및 토크 제어(torque regulation)는 킬로헤르츠(kilohertz) 수준에서 실행될 수 있으며, 위치 또는 속도 제어(position or velocity control)는 수백 헤르츠 수준에서 동작할 수 있다. 상태 추정(state estimation), 인지(perception), 지역 계획(local planning), 학습 정책(learned policy)은 수십에서 수백 헤르츠 수준에서 실행될 수 있는 반면, 의미론적 추론(semantic reasoning), 장기 계획(long-horizon planning), 월드 모델 예측(world-model prediction), 비전-언어 추론(vision-language reasoning)은 더 느린 속도로 동작할 수 있다. 실시간 아키텍처(real-time architecture)는 모든 기능에 동일한 갱신 주기를 강제하지 않으면서 이러한 서로 다른 시간 척도(temporal scales)를 조정해야 한다.

중요한 측정값은 개별 인공지능 모델(AI model)의 추론시간보다 센싱-투-액션 지연시간(sensing-to-action latency)인 경우가 많다. 물리적 사건은 먼저 센서(sensor)에 의해 포착되고, 인터페이스(interface)를 통해 전달되고, 시간 동기화(time synchronization)와 전처리(preprocessing)를 거친 뒤, 인지 시스템(perception system)에 의해 해석되어야 한다. 이후 시스템 상태(system state)에 통합되고, 계획(planning)을 통해 평가되고, 행동 명령(action command)으로 변환되어 최종적으로 액추에이터(actuator)에 의해 실행된다. 각 단계가 지연을 발생시키므로 신경망(neural network)만 최적화한다고 해서 전체 로봇이 실시간 시스템(real-time system)이 되는 것은 아니다.

실시간 요구사항(real-time requirements)은 높은 평균 계산 처리량(average computational throughput)뿐만 아니라 마감시간(deadline)과도 관련된다. GPU가 전체적으로 초당 수백 개의 프레임을 처리하더라도 간헐적으로 매우 느린 추론이 발생할 수 있다. 물리 시스템(physical system)이 특정 시점에 결과를 요구하는 상황에서는 이러한 타이밍 변동(timing variation)이 위험할 수 있다. 따라서 물리 인공지능은 평균 추론 성능뿐만 아니라 최악 조건 또는 높은 백분위 지연시간(high-percentile latency), 스케줄링 지연(scheduling delay), 메모리 경합(memory contention), 센서 전송시간(sensor transfer time), 운영체제 동작(operating-system behavior), 가속기 활용률(accelerator utilization), 통신 오버헤드(communication overhead)를 함께 고려해야 한다.

이러한 특성은 하드 실시간(hard real-time)과 소프트 실시간(soft real-time)의 구분으로 이어진다. 하드 실시간 기능(hard real-time function)은 마감시간을 위반하면 허용할 수 없는 물리적 결과가 발생할 수 있기 때문에 결정론적 실행(deterministic execution)이 필수적이다. 저수준 안정화(low-level stabilization), 비상 정지(emergency stopping), 액추에이터 보호(actuator protection), 일부 안전 모니터링(safety monitoring) 기능이 이러한 범주에 가깝다. 반면 소프트 실시간 기능(soft real-time function)은 가끔 마감시간을 초과하더라도 즉각적인 실패 대신 성능 품질이 저하되는 것을 허용할 수 있다. 의미론적 이해(semantic understanding), 지도 갱신(map update), 전역 최적화(global optimization), 일부 고수준 인공지능 추론(high-level AI reasoning)은 일반적으로 보다 유연한 타이밍 제약(timing constraints)을 갖도록 설계된다.

이러한 이유로 하드웨어-소프트웨어 공동 설계(hardware-software co-design)에서는 결정론적 제어 연산(deterministic control computation)과 계산 집약적인 인공지능 처리(computationally intensive AI processing)를 분리하는 경우가 많다. MCU, 실시간 CPU(real-time CPU), FPGA 또는 전용 제어기(dedicated controller)가 안전 중요 제어 루프(safety-critical control loop)를 유지하는 동안 GPU 또는 NPU는 인지, 학습 정책, 월드 모델(world model), 멀티모달 추론(multimodal reasoning)을 수행할 수 있다. 인공지능 프로세서(AI processor)는 상위 수준 명령이나 기준값을 제공하지만, 인공지능 추론이 지연되더라도 하위 수준 제어기(low-level controller)는 안정적인 물리적 동작을 계속 유지한다. 이러한 분리는 가변적인 인공지능 워크로드(variable AI workload)가 빠른 제어 루프를 직접 불안정하게 만드는 것을 방지한다.

실시간 인지(real-time perception)는 센서 정보(sensor information)가 로봇과 환경의 움직임에 따라 빠르게 오래된 정보가 될 수 있기 때문에 또 다른 중요한 제약을 발생시킨다. 매우 정확한 객체 검출기(object detector)라도 그 결과가 수백 밀리초 이전의 장면을 설명한다면 가치가 제한된다. 카메라 노출(camera exposure), LiDAR 스캐닝, 센서 전송(sensor transmission), 전처리, 신경망 추론(neural inference), 센서 융합(sensor fusion), 추적(tracking)은 모두 정보의 시간적 노후화(information age)를 증가시킨다. 따라서 물리 인공지능은 모델 정확도(model accuracy)뿐만 아니라 해당 결정이 액추에이터에 도달했을 때 표현된 월드 상태(world state)가 얼마나 오래된 것인지도 평가해야 한다.

월드 모델(world model)과 고급 추론(advanced reasoning)은 타이밍 문제를 더욱 복잡하게 만든다. 예측 모델(predictive model)은 관측(observation)과 행동(action) 사이에서 환경이 어떻게 변화할지를 추정함으로써 지연을 부분적으로 보상할 수 있지만, 예측 자체도 계산 자원을 소비하고 불확실성(uncertainty)을 발생시킨다. 따라서 유용한 아키텍처는 반응형 지능(reactive intelligence)과 예측형 지능(predictive intelligence)의 균형을 유지해야 한다. 빠른 경로(fast pathway)는 즉각적인 위험과 상태 변화에 대응하고, 느린 경로(slow pathway)는 시간 중요 동작(time-critical behavior)을 방해하지 않으면서 보다 풍부한 표현을 구성하고, 대안적인 미래를 평가하며, 장기 목표(longer-horizon goal)를 갱신한다.

계획(planning) 역시 후보 궤적(candidate trajectory)을 평가하는 동안 물리 세계가 멈추지 않기 때문에 제한된 계산 시간(computational horizon)을 갖는다. 궤적 수, 최적화 반복 횟수(optimization iteration), 시뮬레이션 롤아웃(simulation rollout), 추론 토큰(reasoning token)을 증가시키면 의사결정 품질(decision quality)을 향상시킬 수 있지만, 이는 계산 시간이 허용된 의사결정 시간창(decision window)을 넘지 않는 범위에서만 유효하다. 따라서 물리 인공지능은 애니타임 계산 문제(anytime-computation problem)를 갖는다. 시스템은 신속하게 사용 가능한 행동을 생성하고 추가적인 계산 시간이 주어질 경우 그 행동을 개선할 수 있어야 하며, 무한정 긴 최적화 과정에 의존해서는 안 된다.

통신 지연(communication latency)은 계산이 제어기(controller), 엣지 컴퓨터(edge computer), 온프레미스 서버(on-premise server), 다른 로봇 또는 클라우드 인프라(cloud infrastructure)에 분산될 때 실시간 아키텍처의 일부가 된다. 네트워크 지연(network delay), 패킷 손실(packet loss), 혼잡(congestion), 일시적인 연결 단절(disconnection) 때문에 즉각적인 반응이 보장되어야 하는 기능을 원격 계산(remote computation)에 의존하는 것은 적절하지 않다. 따라서 안전 중요 인지(safety-critical perception)와 제어는 로컬에서 실행 가능해야 하며, 시간 민감도가 낮은 학습(training), 플릿 학습(fleet learning), 대규모 모델 추론(large-model reasoning), 이력 분석(historical analysis), 모델 관리(model management)는 로봇에서 더 멀리 떨어진 계산 계층에 배치할 수 있다.

지터(jitter)는 절대적인 지연시간(absolute latency)만큼 중요할 수 있다. 항상 40밀리초마다 결과를 생성하는 인지 파이프라인(perception pipeline)은 15밀리초에서 120밀리초 사이로 불규칙하게 변동하는 시스템보다 통합하기 쉽다. 가변적인 타이밍(variable timing)은 센서 정보의 실질적인 정보 연령(information age)을 변화시키며 상태 추정, 예측, 계획, 제어를 복잡하게 만든다. 따라서 타임스탬핑(timestamping), 클록 동기화(clock synchronization), 제한된 큐(bounded queue), 우선순위 스케줄링(priority scheduling), 비동기 파이프라인(asynchronous pipeline), 버퍼링 정책(buffering policy), 결정론적 통신 메커니즘(deterministic communication mechanism)은 실시간 물리 인공지능 엔지니어링의 핵심 요소이다.

큐 관리(queue management)는 과부하된 파이프라인이 개별 알고리즘의 실행속도가 빠르더라도 전체 지연시간을 지속적으로 증가시킬 수 있기 때문에 특히 중요하다. 센서 프레임(sensor frame)이 처리 가능한 속도보다 빠르게 입력되는 경우 모든 프레임을 처리하려 하면 시스템이 점점 더 오래된 세계 상태를 기반으로 판단하게 될 수 있다. 실시간 시스템은 대신 오래된 관측(stale observation)을 폐기하고, 최신 데이터를 우선 처리하며, 모델 복잡도(model complexity)를 낮추거나, 센서 해상도(sensor resolution)를 줄이고, 계획 범위(planning horizon)를 단축하거나, 중요하지 않은 워크로드(nonessential workload)를 일시적으로 중지할 수 있다. 처리되는 정보의 양을 최대화하는 것보다 정보의 최신성(freshness)을 유지하는 것이 더 중요할 수 있다.

따라서 계산 과부하(compute overload)는 통제되지 않는 마감시간 실패(deadline failure)가 아니라 점진적 성능 저하(graceful degradation)로 이어져야 한다. 로봇은 대형 인지 모델에서 소형 모델로 전환하고, 추론 빈도(inference frequency)를 낮추며, 월드 모델 예측을 단순화하고, 최적화 시간을 단축하거나, 카메라 해상도를 줄이고, 백그라운드 매핑(background mapping)을 중지하거나, 보수적인 이동 제한(conservative motion limit)을 활성화할 수 있다. 목표는 계산 품질을 단계적으로 희생하면서 가장 안전에 중요한 기능을 유지하는 것이다. 따라서 실시간 자원 관리(real-time resource management)는 단순한 구현 세부사항이 아니라 지능적 행동(intelligent behavior)의 능동적인 구성요소가 된다.

이러한 시스템을 설계하려면 전체 센싱-인지-추론-계획-제어(sensing-perception-reasoning-planning-control) 체인에 걸쳐 명시적인 지연시간 예산(latency budget)을 설정해야 한다. 각 서브시스템(subsystem)에는 허용 가능한 응답시간의 일부가 할당되며, 여기에는 스케줄링, 데이터 이동(data movement), 동기화, 통신 및 예상치 못한 계산 변동을 위한 여유시간도 포함된다. 이후 프로파일링(profiling)은 격리된 벤치마크 결과에만 의존하지 않고 현실적인 워크로드(realistic workload)에서 배포된 시스템을 측정해야 한다. CPU, GPU, 메모리, 네트워크, 센서, 액추에이터의 타이밍을 함께 평가해야 하며, 이들의 상호작용이 실제 물리적 응답(physical response)을 결정하기 때문이다.

궁극적으로 실시간 물리 인공지능(real-time Physical AI)은 모든 계산을 최대한 빠르게 만드는 것으로 정의되지 않는다. 핵심은 계산 타이밍(computational timing)을 물리적 동역학(physical dynamics)에 맞추고, 필수적인 지능적 결과가 그 유효성을 잃기 전에 도달하도록 보장하는 것이다. 아키텍처는 빠르고 결정론적인 제어(fast deterministic control), 제한된 지연시간의 인지(bounded-latency perception), 시간 인식형 예측 및 계획(time-aware prediction and planning), 비동기 고수준 추론(asynchronous high-level reasoning), 과부하 상황에서의 안전한 성능 저하(safe degradation)를 결합해야 한다. 따라서 실시간 능력(real-time capability)은 센서, 컴퓨팅 하드웨어, 소프트웨어 스케줄링, 인공지능 모델, 네트워크, 제어기, 액추에이터를 조화롭게 설계함으로써 나타나는 시스템 수준 특성(system-level property)이다.

## 06.02. Sensing to Action Latency

![](images/image2.png){width="7.268055555555556in" height="7.268055555555556in"}

물리 인공지능(Physical AI)에서 센싱-투-액션 지연시간(sensing-to-action latency)은 물리적 사건이 관측된 시점부터 이에 대응하는 물리적 반응이 로봇에 의해 생성되는 시점까지의 전체 경과 시간을 의미한다. 이는 단일 신경망(neural network)의 실행시간이 아니라 종단간(end-to-end) 시스템 특성이다. 카메라 노출(camera exposure), 센서 스캐닝(sensor scanning), 통신(communication), 전처리(preprocessing), 인지(perception), 상태 추정(state estimation), 추론(reasoning), 계획(planning), 명령 생성(command generation), 제어(control), 액추에이터 응답(actuator response)이 모두 물리 시스템이 경험하는 지연에 영향을 준다.

유용한 지연시간 모델(latency model)은 정보가 물리적으로 획득되는 순간부터 시작한다. 카메라는 노출 및 판독 시간(exposure and readout time)이 필요하고, LiDAR 센서는 스캐닝 과정에서 측정값을 누적하며, 레이더(radar)는 신호 처리(signal processing)를 수행하고, 관성 센서(inertial sensor)는 유한한 시간 간격으로 움직임을 샘플링한다. 따라서 센서 데이터가 컴퓨팅 시스템에 입력되는 시점에는 이미 약간 이전의 물리적 상태를 나타내고 있다. 그러므로 타임스탬프 정확도(timestamp accuracy)와 동기화(synchronization)는 각 관측이 실제 물리 세계의 어느 시점을 나타내는지 판단하는 데 필수적이다.

데이터 획득(acquisition) 이후에는 인공지능 처리가 시작되기 전에 센서 정보가 인터페이스(interface)와 메모리를 거쳐 이동해야 한다. 이더넷(Ethernet), CAN, USB, PCIe, MIPI, 공유 메모리(shared memory), DMA, 디바이스 드라이버(device driver), 미들웨어(middleware), 운영체제 큐(operating-system queue)는 추가적인 지연을 발생시킬 수 있다. 대용량 카메라 영상이나 고밀도 포인트 클라우드(dense point cloud)는 상당한 대역폭(bandwidth)을 소비할 수도 있다. 따라서 하드웨어-소프트웨어 공동 설계(hardware-software co-design)는 통신과 메모리 전송을 무시할 수 있는 오버헤드가 아니라 전체 지연 경로(latency path)의 일부로 고려해야 한다.

전처리(preprocessing)는 원시 센서 측정값(raw sensor measurements)을 후속 계산에 적합한 표현으로 변환한다. 영상 크기 조정(image resizing), 영상 보정(rectification), 정규화(normalization), 포인트 클라우드 필터링(point-cloud filtering), 복셀화(voxelization), 레이더 처리(radar processing), 좌표 변환(coordinate transformation), 시간 정렬(temporal alignment)은 각각 사용 가능한 응답시간의 일부를 소비할 수 있다. 일부 연산은 병렬로 실행하거나 가속기(accelerator)에서 직접 처리할 수 있지만, 잘못 설계된 파이프라인은 CPU와 GPU 메모리 사이에서 데이터를 반복적으로 복사한다. 따라서 효율적인 전처리에는 알고리즘 최적화(algorithmic optimization)와 신중한 연산 배치(computation placement)가 모두 필요하다.

인지 지연시간(perception latency)은 인공지능 모델이 센서 데이터를 환경에 대한 정보로 변환할 때 발생한다. 객체 검출(object detection), 분할(segmentation), 깊이 추정(depth estimation), 점유 예측(occupancy prediction), 추적(tracking), 위치 추정(localization), 멀티모달 융합(multimodal fusion)은 순차적으로 또는 동시에 실행될 수 있다. 대형 모델은 표현 능력(representational capability)을 향상시킬 수 있지만 일반적으로 더 많은 계산을 요구한다. 따라서 물리 인공지능에 가장 적합한 모델은 단독 평가에서 가장 높은 정확도를 보이는 모델이 아니라, 요구되는 시간 범위(temporal window) 안에서 충분히 신뢰할 수 있는 세계 정보를 생성하는 모델이다.

상태 추정(state estimation)은 서로 다른 센서의 관측값이 동시에 도착하는 경우가 드물기 때문에 또 다른 시간적 문제를 발생시킨다. 카메라, LiDAR, 레이더, IMU, 휠 인코더(wheel encoder), GNSS는 서로 다른 주기와 전송 지연(transport delay)으로 동작할 수 있다. 시스템은 측정값을 적절한 타임스탬프(timestamp)와 연결하고 비동기 관측(asynchronous observation)으로부터 현재 상태를 추정해야 한다. 시간 정렬(temporal alignment)이 제대로 이루어지지 않으면 공간적으로 정확한 측정값이라도 로봇 움직임의 서로 다른 시점에 해당하기 때문에 잘못된 표현을 생성할 수 있다.

월드 모델링(world modeling)과 추론(reasoning)은 인지와 의사결정 사이에 추가적인 지연을 발생시킨다. 월드 모델(world model)은 시간에 따라 관측값을 통합하고, 숨겨진 상태(hidden state)를 추정하며, 미래 움직임을 예측하고, 불확실성(uncertainty)을 평가하거나 가능한 미래 시나리오를 생성할 수 있다. 보다 정교한 추론은 의사결정 품질(decision quality)을 향상시킬 수 있지만 계산 비용은 물리적 마감시간(physical deadline)과 양립할 수 있어야 한다. 예측 능력(predictive capability)은 계산 및 액추에이션 지연이 발생하는 동안 세계가 어떻게 변화할지를 시스템이 추정할 수 있다는 점에서 특히 중요하다.

계획(planning)은 추정되거나 예측된 세계 상태(world state)를 의도된 행동으로 변환한다. 지역 궤적 생성(local trajectory generation), 충돌 검사(collision checking), 최적화(optimization), 행동 선택(behavior selection), 모델 예측 제어(model predictive control), 학습 정책(learned policy)은 하나의 행동을 선택하기 전에 여러 후보 행동을 평가할 수 있다. 탐색 깊이(search depth)나 최적화 반복 횟수를 증가시키면 해결책을 개선할 수 있지만 응답시간도 길어진다. 따라서 실제 물리 인공지능 시스템에는 제한된 계산(bounded computation), 조기 종료(early termination), 계층적 계획(hierarchical planning), 또는 마감시간 이전에 사용 가능한 행동을 제공할 수 있는 애니타임 알고리즘(anytime algorithm)이 필요하다.

선택된 행동은 이후 명령 생성(command generation) 및 제어 계층(control layer)을 통과해야 한다. 전진 이동, 궤적 추종(trajectory following), 목표 자세 도달(reach a pose), 힘 적용(apply a force)과 같은 고수준 명령(high-level command)은 속도, 위치, 토크, 전류 또는 기타 액추에이터 수준 기준값(actuator-level reference)으로 변환되어야 한다. 이러한 변환에는 운동학(kinematics), 동역학(dynamics), 제약조건 처리(constraint handling), 보간(interpolation), 안전 검사(safety check), 임베디드 제어기(embedded controller)와의 통신이 포함될 수 있다. 따라서 명령 경로(command path) 역시 전체 센싱-투-액션 지연시간의 구성요소이다.

액추에이션(actuation) 자체도 즉각적으로 이루어지지 않는다. 모터는 토크를 발생시키기 위해 전류가 형성되는 시간이 필요하고, 변속기(transmission)는 기계적 동역학(mechanical dynamics)을 발생시키며, 유압 또는 공압 시스템(hydraulic or pneumatic system)은 압력 동역학(pressure dynamics)을 갖는다. 또한 로봇 구조는 관성(inertia), 컴플라이언스(compliance), 마찰(friction), 백래시(backlash)의 영향을 받는다. 따라서 센싱-투-액션 지연시간의 의미 있는 종료 시점은 항상 소프트웨어가 명령을 전송한 순간은 아니다. 많은 응용에서 실제 종료 시점은 명령된 물리적 반응이 현실 세계에서 실제로 시작되거나 요구 수준에 도달하는 순간이다.

전체 지연시간(total latency)은 개념적으로 센싱(sensing), 전송(transfer), 전처리, 인지, 추정(estimation), 추론, 계획, 명령, 제어, 통신 및 액추에이터 지연의 합으로 표현할 수 있다. 그러나 실제 시스템은 파이프라인(pipeline)으로 동작하기 때문에 단순한 합산만으로 동작을 완벽하게 설명할 수 있는 것은 아니다. 작업이 서로 중첩될 수 있고, 센서가 비동기적으로 동작할 수 있으며, GPU 커널(GPU kernel)이 동시에 실행되거나 제어기가 사용 가능한 최신 상태를 이용할 수도 있다. 따라서 지연시간 분석(latency analysis)은 각 모듈의 명목상 실행시간만이 아니라 실제 실행 의존성(execution dependency)을 분석해야 한다.

정보 연령(information age)은 센싱-투-액션 지연시간과 밀접하게 관련되어 있다. 예를 들어 카메라 프레임이 시간 t에서 세계를 나타내지만 이에 따른 액추에이터 응답이 t 이후 150밀리초에 발생한다고 가정할 수 있다. 이 경우 행동은 이미 상당히 오래된 정보에 기반할 수 있다. 로봇이나 주변 물체가 빠르게 움직인다면 정확한 인지 결과도 잘못된 행동으로 이어질 수 있다. 따라서 물리 인공지능은 센싱 시점에 관측된 상태만이 아니라 실제 행동 시점(action time)에 예상되는 상태를 기반으로 판단해야 한다.

지연시간과 로봇 속도(robot velocity)는 물리적 변위(physical displacement)를 통해 직접적으로 상호작용한다. 지연이 발생하는 동안 이동 플랫폼(moving platform)은 계속 주행하고, 매니퓰레이터는 계속 움직이며, 주변 물체도 위치를 변경할 수 있다. 시스템의 이동 속도가 빠를수록 동일한 시간 지연으로 발생하는 공간 오차(spatial error)는 커진다. 이러한 관계로 인해 지연시간은 물리적 안전 변수(physical safety parameter)가 된다. 따라서 최대 속도(maximum speed), 제동 거리(braking distance), 장애물 감지 거리(obstacle detection range), 제어 주파수(control frequency), 예측 범위(prediction horizon), 계산 지연(computational delay)은 서로 독립적으로가 아니라 함께 설계되어야 한다.

평균 지연시간(average latency)만으로는 센싱-투-액션 경로의 특성을 충분히 설명할 수 없다. 평균 50밀리초이지만 간헐적으로 200밀리초가 필요한 시스템은 항상 70밀리초 안에 일관되게 반응하는 시스템보다 적합하지 않을 수 있다. 지터(jitter)는 운영체제 스케줄링(operating-system scheduling), 메모리 할당(memory allocation), 캐시 효과(cache effect), GPU 경합(GPU contention), 네트워크 혼잡(network congestion), 큐 누적(queue accumulation), 열 스로틀링(thermal throttling), 동시 인공지능 워크로드(simultaneous AI workload)로 인해 발생할 수 있다. 따라서 실시간 물리 인공지능은 지연시간 분포(latency distribution), 백분위수(percentile), 최악 조건 동작(worst-case behavior), 마감시간 위반율(deadline-miss rate)을 측정해야 한다.

큐잉(queueing)은 계산 과부하(compute overload)를 지속적으로 증가하는 정보 연령으로 변환할 수 있기 때문에 특히 위험하다. 카메라가 인지 시스템이 처리할 수 있는 속도보다 빠르게 프레임을 생성한다면 일반적인 선입선출 파이프라인(first-in-first-out pipeline)은 모든 프레임을 처리하면서 현실보다 점점 더 뒤처질 수 있다. 실시간 자율 시스템(real-time autonomy)에서는 오래된 프레임(stale frame)을 폐기하고 가장 최신 관측값을 처리하는 것이 더 적절할 수 있다. 목표는 반드시 모든 샘플을 처리하는 것이 아니라 현재 물리적 상태에 대한 정확하고 시의성 있는 추정(timely estimate)을 유지하는 것이다.

센싱-투-액션 지연시간을 줄이기 위해서는 전체 시스템에 걸친 최적화가 필요하다. 빠른 센서만으로 느린 추론을 보완할 수 없으며, 빠른 GPU만으로 비효율적인 데이터 전송이나 액추에이터 동역학을 제거할 수도 없다. 제로카피 파이프라인(zero-copy pipeline), 하드웨어 가속(hardware acceleration), 병렬 실행(parallel execution), 비동기 처리(asynchronous processing), 모델 압축(model compression), 최적화된 미들웨어(optimized middleware), 실시간 스케줄링(real-time scheduling), 동기화된 클록(synchronized clock), 제한된 큐(bounded queue), 효율적인 제어 인터페이스(control interface), 예측 상태 추정(predictive state estimation)이 모두 지연시간 감소에 기여할 수 있다. 이러한 기술의 효과는 종단간 시스템 수준에서 평가해야 한다.

서로 다른 경로(pathway)에 서로 다른 지연시간 요구사항을 할당할 수도 있다. 비상 충돌 회피(emergency collision avoidance)는 짧고 결정론적인 센서-투-제어기 경로(sensor-to-controller path)를 사용할 수 있는 반면, 보다 풍부한 인지와 월드 모델 추론은 상대적으로 느린 인공지능 파이프라인을 통해 실행될 수 있다. 고수준 지능(high-level intelligence)은 즉각적인 안전 반응을 방해하지 않으면서 목표와 예측을 지속적으로 개선할 수 있다. 이러한 계층적 구성(hierarchical arrangement)을 통해 물리 인공지능은 정교한 계산과 빠른 물리적 반응을 결합할 수 있으며, 이는 하드웨어-소프트웨어 공동 설계에서 인지, 추론, 계획, 제어 및 이기종 컴퓨팅(heterogeneous computing)을 분리하는 보다 광범위한 구조를 반영한다.

궁극적으로 센싱-투-액션 지연시간은 디지털 지능(digital intelligence)이 얼마나 빠르게 의미 있는 물리적 행동(physical behavior)으로 전환되는지를 결정한다. 로봇은 계산이 완료된 순간의 세계에 행동하는 것이 아니라 센싱과 계산이 진행되는 동안에도 계속 변화해 온 세계에 행동한다. 따라서 성공적인 물리 인공지능은 불필요한 지연을 최소화하고, 타이밍 변동(timing variability)을 제한하며, 피할 수 없는 지연 동안 상태 변화를 예측하고, 빠른 안전 경로(fast safety pathway)를 유지해야 한다. 종단간 지연시간(end-to-end latency)은 센서, 인공지능 모델, 컴퓨팅 플랫폼(compute platform), 통신, 제어기, 액추에이터 및 물리적 동역학을 연결하는 근본적인 아키텍처 제약조건(architectural constraint)으로 다루어져야 한다.

## 06.03. Perception Latency

![](images/image3.png){width="7.268055555555556in" height="7.268055555555556in"}

인지 지연시간(perception latency)은 물리 인공지능(Physical AI) 시스템이 센서 관측(sensor observation)을 주변 세계에 대한 행동 가능한 표현(actionable representation)으로 변환하는 데 필요한 시간이다. 이는 센서 데이터가 인지 파이프라인(perception pipeline)에 제공된 이후 시작되며, 전처리(preprocessing), 신경망 추론(neural inference), 센서 융합(sensor fusion), 검출(detection), 분할(segmentation), 추적(tracking), 깊이 추정(depth estimation), 위치 추정(localization) 등의 해석 단계를 거쳐 완료된다. 따라서 전체 지연시간 아키텍처(latency architecture)에서 인지는 센싱(sensing)을 월드 모델링(world modeling), 계획(planning), 제어(control)와 연결하는 핵심 구성요소이다.

기존의 오프라인 컴퓨터 비전(offline computer vision)과 달리 물리 인공지능의 인지는 정확도(accuracy)와 타이밍 요구사항(timing requirements)을 동시에 충족해야 한다. 매우 정확한 결과라도 물리적 장면이 크게 변화한 이후에 도착한다면 실질적인 가치가 떨어질 수 있다. 인지가 실행되는 동안에도 로봇은 이동하고, 장애물의 위치는 변하며, 사람이 작업 공간에 들어오거나 나가고, 매니퓰레이터(manipulator)는 물체와 계속 상호작용한다. 따라서 인지 품질(perception quality)은 기존의 정확도 지표뿐만 아니라 결과가 실제로 언제 사용 가능해지는지와 함께 평가되어야 한다.

인지 지연시간은 신경망 추론시간(neural-network inference time)만으로 구성되지 않는다. 원시 영상(raw image)은 추론 전에 디코딩(decoding), 크기 조정(resizing), 영상 보정(rectification), 정규화(normalization), 색상 변환(color conversion)이 필요할 수 있다. LiDAR 포인트 클라우드(point cloud)는 필터링, 좌표 변환(coordinate transformation), 복셀화(voxelization), 거리 영상 생성(range-image construction)이 필요할 수 있다. 레이더 측정값은 신호 처리(signal processing)와 클러스터링(clustering)을 거칠 수 있다. 추론 이후에도 출력 결과가 후속 의사결정에 사용되기 전에 디코딩, 비최대 억제(non-maximum suppression), 기하학적 변환(geometric transformation), 연관(association), 추적 또는 융합 과정을 거칠 수 있다.

센서 모달리티(sensor modality)는 인지 파이프라인의 구조에 큰 영향을 준다. 카메라는 밀도 높은 시각 정보(dense visual information)를 제공하지만 상당한 영상 처리 및 신경망 추론 워크로드를 발생시킬 수 있다. LiDAR는 직접적인 기하학적 측정값을 제공하지만 공간 처리가 필요한 포인트 클라우드를 생성한다. 레이더는 서로 다른 해상도 특성을 가지면서 유용한 속도 및 거리 정보를 제공하고, IMU와 고유수용성 센서(proprioceptive sensor)는 훨씬 높은 갱신 주기(update rate)로 동작한다. 멀티모달 물리 인공지능(multimodal Physical AI)은 동기화 과정에서 과도한 대기시간을 발생시키지 않으면서 이러한 비동기 정보 스트림(asynchronous information stream)을 결합해야 한다.

모델 아키텍처(model architecture)는 인지 지연시간을 결정하는 가장 큰 요인 중 하나이다. 영상 해상도(image resolution), 백본 크기(backbone size), 트랜스포머 깊이(transformer depth), 특징 차원(feature dimension), 검출 쿼리(detection query), 시간적 문맥(temporal context), 센서 뷰(sensor view)의 수를 증가시키면 인지 능력이 향상될 수 있지만 계산량과 메모리 트래픽(memory traffic)도 증가한다. 따라서 물리 인공지능의 모델 선택은 정확도-지연시간 절충(accuracy-latency tradeoff)을 고려해야 한다. 최적의 모델은 흔히 안전한 후속 의사결정에 필요한 정보를 주어진 마감시간(deadline) 내에서 안정적으로 제공할 수 있는 가장 작거나 효율적인 아키텍처이다.

메모리 이동(memory movement)은 산술 연산(arithmetic computation)만큼 중요해질 수 있다. 센서 데이터는 디바이스 버퍼(device buffer)에서 시스템 메모리로, CPU 메모리에서 GPU 메모리로 이동하고, 여러 중간 텐서(intermediate tensor)를 거친 뒤 다시 다른 프로세서나 제어기로 전달될 수 있다. 대규모 멀티카메라 영상과 고밀도 3차원 표현은 이러한 비용을 더욱 증가시킨다. 제로카피 전송(zero-copy transfer), 고정 메모리(pinned memory), 공유 버퍼(shared buffer), 효율적인 텐서 레이아웃(tensor layout), 가속기 상주 전처리(accelerator-resident preprocessing), CPU-GPU 동기화 최소화는 기본 인공지능 모델을 변경하지 않고도 지연시간을 감소시킬 수 있다.

병렬 처리(parallelism)는 실질적인 인지 지연시간을 줄이는 또 다른 중요한 방법이다. 여러 카메라 스트림을 동시에 처리할 수 있고, 센서 데이터 획득과 전처리를 중첩할 수 있으며, 다음 추론이 시작되는 동안 추적을 수행할 수도 있다. 서로 다른 신경망이 가속기를 공유하거나 별도의 컴퓨팅 장치에서 실행될 수도 있다. 그러나 과도한 동시 실행은 GPU 경합(GPU contention), 메모리 대역폭 포화(memory-bandwidth saturation), 캐시 간섭(cache interference), 큐잉(queueing)을 발생시켜 평균 처리량은 높더라도 최악 조건 지연시간(worst-case latency)을 증가시킬 수 있으므로 신중한 스케줄링이 필요하다.

배치 처리(batch processing)는 처리량(throughput)과 지연시간(latency)의 차이를 잘 보여준다. 여러 영상을 하나의 추론 배치(inference batch)로 결합하면 GPU 활용률과 초당 전체 프레임 처리량을 향상시킬 수 있지만, 배치가 구성될 때까지 기다리는 시간이 개별 관측의 처리를 지연시킬 수 있다. 오프라인 인공지능은 대규모 배치의 이점을 얻는 경우가 많지만 실시간 물리 인공지능(real-time Physical AI)은 흔히 소규모 배치, 스트리밍 실행(streaming execution), 즉시 처리(immediate processing)를 선호한다. 올바른 구성은 전체 처리량과 각각의 물리적 사건에 대한 적시 대응 중 어느 것이 주요 목표인지에 따라 달라진다.

시간적 인지(temporal perception)는 추가적인 설계 선택을 요구한다. 추적, 비디오 모델(video model), 순환 신경망(recurrent network), 시간적 트랜스포머(temporal transformer), 점유 예측(occupancy prediction), 월드 상태 추정(world-state estimation)은 여러 시간 단계의 정보를 활용하여 강건성(robustness)과 일관성을 향상시킬 수 있다. 그러나 미래 관측을 기다리거나 긴 이력(history)을 처리하면 계산 비용과 시간적 비용이 증가한다. 따라서 즉각적인 물리적 행동이 인지 결과에 의존하는 경우에는 미래 프레임을 기다리지 않고 사용 가능한 과거 정보를 활용하는 인과적 아키텍처(causal architecture)가 일반적으로 더 적합하다.

센서 융합(sensor fusion)은 불확실성을 감소시킬 수 있지만 동기화 지연(synchronization latency)을 발생시킬 수도 있다. 융합 모듈(fusion module)은 카메라, LiDAR, 레이더 및 고유수용성 측정값이 거의 동일한 시점에 대응하도록 데이터가 도착할 때까지 기다릴 수 있다. 이러한 대기는 시간적 정렬(temporal alignment)을 향상시키지만 융합 결과를 지연시킨다. 반대로 비동기 융합(asynchronous fusion)은 움직임 추정(motion estimation)이나 상태 예측(state prediction)을 사용하여 측정값을 공통 기준시간(common reference time)으로 전파할 수 있다. 이는 블로킹(blocking)을 줄이지만 더욱 정교한 추정과 타이밍 불확실성(timing uncertainty)의 명시적 처리를 요구한다.

인지 정보의 연령(age of perception information)은 궁극적으로 단독으로 평가된 추론 지연시간보다 중요하다. 신경망이 단지 20밀리초만 필요하더라도 입력 영상은 노출, 판독, 전송, 큐잉 때문에 이미 30밀리초 이전의 정보일 수 있다. 추가적인 후처리(postprocessing)와 스케줄링 지연이 발생하면 최종 표현은 훨씬 더 오래된 정보가 될 수 있다. 따라서 모든 인지 출력은 단순히 계산이 완료된 시각이 아니라 그 결과의 기반이 된 관측이 실제로 어느 시점을 나타내는지와 연결되어야 한다.

이러한 구분은 움직이는 객체(moving object)를 처리할 때 특히 중요하다. 차량, 이동 로봇(mobile robot), 사람 또는 매니퓰레이터가 인지 연산 중 빠르게 움직이고 있다고 가정하면, 객체 검출기(object detector)가 위치를 보고하는 시점에는 해당 객체가 더 이상 그 위치에 존재하지 않을 수 있다. 추적 및 움직임 추정은 검출된 상태를 현재 시점으로 투영함으로써 이를 보상할 수 있다. 더욱 발전된 시스템은 예상 행동 시점(expected action time)까지 상태를 예측할 수 있으며, 이를 통해 계획 시스템은 오래된 스냅샷(stale snapshot)이 아니라 시간적으로 보정된 표현(temporally corrected representation)을 기반으로 동작할 수 있다.

인지 주파수(perception frequency)와 인지 지연시간은 서로 관련되어 있지만 서로 다른 개념이다. 30Hz로 실행되는 검출기는 약 33밀리초마다 새로운 결과를 생성하지만 각각의 결과는 데이터 획득부터 완료까지 실제로 50밀리초가 걸릴 수도 있다. 반대로 깊게 파이프라인화된 시스템(deeply pipelined system)은 높은 처리량을 달성하면서도 프레임별 지연시간(per-frame latency)은 상당히 클 수 있다. 따라서 물리 인공지능 아키텍트는 초당 프레임 수(frames per second)를 완전한 실시간 성능 지표로 간주하지 말고 갱신 주기, 종단간 지연(end-to-end delay), 정보 연령, 지터(jitter)를 각각 측정해야 한다.

지터는 인지 지연시간의 예측 가능성을 떨어뜨린다. GPU 스케줄링, 동적 메모리 할당(dynamic memory allocation), 운영체제 활동, 열 스로틀링(thermal throttling), 가변적인 장면 복잡도(variable scene complexity), 희소 데이터 특성(sparse-data characteristics), 통신 트래픽, 경쟁하는 모델(competing model)은 실행시간을 변동시킬 수 있다. 제한된 시간 범위 안에서 일관되게 응답하는 인지 시스템은 평균 성능은 더 좋지만 간헐적으로 긴 지연을 발생시키는 시스템보다 안전할 수 있다. 따라서 평균 추론시간과 함께 백분위 지연시간(percentile latency)과 마감시간 위반 확률(deadline-miss probability)이 중요하다.

큐 누적(queue buildup)은 인지 정보의 최신성(perception freshness)을 심각하게 저하시킬 수 있다. 여섯 대의 카메라가 지속적으로 프레임을 생성하는 동안 인지 스택(perception stack)에 일시적인 과부하가 발생하면, 대기 중인 영상이 처리되는 속도보다 빠르게 누적될 수 있다. 대기 중인 모든 프레임을 처리하면 완전성(completeness)은 유지할 수 있지만 로봇은 과거의 세계를 인지하게 될 수 있다. 실시간 시스템에는 일시적인 과부하가 지속적인 센싱-투-의사결정 지연(sensing-to-decision delay)으로 전환되지 않도록 제한된 큐(bounded queue), 최신 프레임 정책(latest-frame policy), 적응형 프레임 건너뛰기(adaptive frame skipping), 워크로드 우선순위화(workload prioritization)가 필요한 경우가 많다.

적응형 인지(adaptive perception)는 시스템 조건에 따라 계산량을 변화시킬 수 있도록 한다. 환경이 단순하거나 로봇이 천천히 움직이는 경우 일부 인지 모듈은 낮은 주파수로 동작할 수 있다. 반대로 속도, 불확실성, 장애물 밀도(obstacle density), 상호작용 복잡도(interaction complexity)가 증가하면 중요 모델에 더 많은 컴퓨팅 자원을 할당할 수 있다. 과부하 상황에서는 필수적인 인지 기능을 유지하면서 영상 해상도를 낮추고, 보조 모델(secondary model)을 비활성화하고, 시간적 문맥을 단축하고, 경량 네트워크(lightweight network)를 선택하거나 안전과 관련된 영역을 우선적으로 처리할 수 있다.

서로 다른 인지 출력(perception output)에 서로 다른 마감시간을 부여할 수도 있다. 장애물 근접도(obstacle proximity), 자유 공간 추정(free-space estimation), 충돌 위험(collision risk), 국부 움직임(local motion)은 빠른 갱신을 요구할 수 있지만, 의미론적 분류(semantic classification), 상세 장면 이해(detailed scene understanding), 장기 지도 작성(long-term mapping), 객체 속성(object attribute)은 상대적으로 느린 처리를 허용할 수 있다. 따라서 계층적 인지 아키텍처(hierarchical perception architecture)는 풍부하지만 느린 의미론적·예측적 경로와 함께 빠른 안전 중심 경로(fast safety-oriented pathway)를 유지할 수 있다. 이를 통해 계산 비용이 높은 환경 이해가 즉각적인 물리적 반응을 방해하는 것을 방지할 수 있다.

예측 가능한 인지 지연시간을 달성하기 위해서는 하드웨어-소프트웨어 공동 설계(hardware-software co-design)가 필수적이다. 센서 인터페이스(sensor interface), 메모리 아키텍처(memory architecture), CPU 전처리, GPU 또는 NPU 추론, 미들웨어, 동기화, 모델 아키텍처, 후속 처리 모듈(downstream consumer)을 하나의 파이프라인으로 설계해야 한다. 프로파일링(profiling)은 계산량이 많은 신경망 계층뿐만 아니라 전송 정체(transfer stall), 동기화 장벽(synchronization barrier), 큐 지연(queue delay), 메모리 경합(memory contention), 후처리 병목(postprocessing bottleneck)도 찾아내야 한다. 모델만 최적화하면 상당한 시스템 수준 지연(system-level latency)이 그대로 남을 수 있다.

궁극적으로 인지 지연시간(perception latency)은 원시 물리적 관측(raw physical observation)이 얼마나 빠르게 기계가 활용할 수 있는 환경 이해(machine understanding)로 변환되는지를 결정한다. 목표는 단순히 초당 프레임 수를 최대화하거나 하나의 벤치마크 추론시간을 최소화하는 것이 아니라, 후속 의사결정이 무효화되기 전에 정확하고 시간적으로 유효하며 예측 가능한 세계 정보를 제공하는 것이다. 따라서 물리 인공지능은 전체 센싱-투-액션 아키텍처(sensing-to-action architecture)의 일부로서 모델 품질(model quality), 정보 최신성(information freshness), 타이밍 결정성(timing determinism), 자원 활용(resource utilization), 동기화, 점진적 성능 저하(graceful degradation)를 함께 최적화하는 인지 시스템을 요구한다.

## 06.04. World Model and Reasoning Latency

![](images/image4.png){width="7.268055555555556in" height="7.268055555555556in"}

![](images/image5.png){width="7.268055555555556in" height="7.268055555555556in"}

월드 모델 및 추론 지연시간(world model and reasoning latency)은 물리 인공지능(Physical AI) 시스템이 인지된 관측(perceived observation)과 추정된 상태(estimated state)를 예측적이고 구조화되며 의사결정에 유용한 지식으로 변환하는 데 필요한 시간이다. 이는 인지(perception)와 계획(planning) 사이의 계산 영역에 위치하며, 로봇은 현재 관측을 메모리(memory), 동역학(dynamics), 불확실성(uncertainty), 목표(goal), 이전 경험(prior experience)과 통합한다. 단순한 반응형 처리(reactive processing)와 달리 이 단계에서는 행동을 선택하기 전에 숨겨진 상태(hidden state)와 가능한 미래를 평가할 수 있다.

월드 모델(world model)은 센서가 현재 관측하는 것만을 표현하는 것이 아니다. 잠재 상태(latent state), 기하학적 구조(geometric structure), 객체 관계(object relationship), 점유 상태(occupancy), 움직임(motion), 의미 정보(semantics), 물리적 속성(physical properties), 시간적 이력(temporal history)을 유지할 수 있다. 새로운 관측이 들어올 때마다 이러한 표현을 갱신하려면 계산이 필요하다. 이에 따른 지연시간은 표현의 복잡도, 입력 모달리티(input modality), 시간적 문맥(temporal context), 모델 아키텍처(model architecture), 메모리 접근(memory access), 각 갱신 시 내부 월드 상태 중 얼마나 많은 부분을 다시 계산해야 하는지에 따라 달라진다.

예측 월드 모델(predictive world model)은 최신 관측 이후 환경이 어떻게 변화할지를 추정하기 때문에 추가적인 계산을 발생시킨다. 미래 객체 움직임, 로봇 상태 전이(robot state transition), 점유 변화(occupancy change), 접촉 사건(contact event), 행동 결과(action consequence)를 여러 시간 단계에 걸쳐 예측할 수 있다. 일반적으로 예측 범위(prediction horizon)가 길고 시간 해상도(temporal resolution)가 높을수록 더 많은 계산이 필요하다. 따라서 물리 인공지능은 예측이 계획과 제어에 유용한 시간 안에 제공될 수 있도록 예측 깊이(prediction depth)와 마감시간(deadline) 사이의 균형을 유지해야 한다.

잠재 월드 모델(latent world model)은 완전한 센서 관측을 재구성하는 대신 압축된 표현 공간(compressed representation space)에서 예측을 수행함으로써 계산 비용을 줄일 수 있다. 모든 미래 영상이나 포인트 클라우드(point cloud)를 생성하는 대신, 기하학, 움직임, 객체 또는 상호작용 정보를 포함하는 작업 관련 잠재 상태(task-relevant latent state)를 전파할 수 있다. 이를 통해 예측 추론(predictive reasoning)을 보다 효율적으로 수행할 수 있지만, 인코딩(encoding), 잠재 상태 갱신(latent-state update), 디코딩(decoding), 불확실성 추정(uncertainty estimation)은 여전히 전체 지연시간에 영향을 준다.

추론 지연시간(reasoning latency)은 월드 상태 예측(world-state prediction)을 넘어선다. 물리 인공지능 시스템은 인과 관계(causal relationship), 작업 제약조건(task constraint), 객체 어포던스(object affordance), 안전 조건(safety condition), 대안 전략(alternative strategy), 다단계 행동 결과(multi-step action consequence)를 평가할 수 있다. 비전-언어 모델(vision-language model), 비전-언어-행동 모델(vision-language-action model), 트랜스포머(transformer), 탐색 절차(search procedure), 계획 중심 파운데이션 모델(planning-oriented foundation model)은 더욱 풍부한 추론 능력을 제공할 수 있지만, 기존 인지 네트워크보다 계산 요구량이 훨씬 크고 변동성도 높을 수 있다.

토큰 기반 추론(token-based reasoning)은 특히 중요한 타이밍 문제를 발생시킨다. 대규모 멀티모달 모델(large multimodal model)은 중간 추론 상태(intermediate reasoning state)나 행동 표현(action representation)을 자기회귀적(autoregressive)으로 생성할 수 있으며, 이에 따라 시퀀스 길이(sequence length)가 증가할수록 지연시간도 증가한다. 더 많은 추론 단계는 잠재적으로 해결책의 품질을 향상시킬 수 있지만 물리 시스템은 추가 토큰을 무한정 기다릴 수 없다. 따라서 추론 깊이(reasoning depth)는 기반이 되는 상태가 환경 변화로 인해 무효화되기 전까지 사용 가능한 시간으로 제한되어야 한다.

월드 모델 지연시간과 추론 지연시간이 반드시 동일한 마감시간을 가져야 하는 것은 아니다. 국부 점유(local occupancy)를 갱신하거나 주변 객체의 움직임을 예측하는 작업은 충돌 회피(collision avoidance)와 지역 계획(local planning)에 직접적인 영향을 주므로 빠르게 실행되어야 할 수 있다. 반면 의미론적 해석(semantic interpretation), 작업 분해(task decomposition), 장기 추론(long-horizon reasoning), 언어 기반 계획(language-based planning)은 상대적으로 느리게 실행될 수 있다. 따라서 계층적 아키텍처(hierarchical architecture)는 월드 모델링과 추론을 빠른, 중간, 느린 시간 경로(temporal pathway)로 구분할 수 있다.

빠른 월드 모델 경로(fast world-model pathway)는 즉시 행동에 활용할 수 있는 정보에 중점을 두어야 한다. 국부 기하학(local geometry), 동적 점유(dynamic occupancy), 자차 움직임(ego motion), 장애물 궤적(obstacle trajectory), 접촉 상태(contact state), 주행 가능성(traversability), 단기 예측(short-horizon prediction)을 비교적 높은 주파수로 갱신할 수 있다. 이러한 표현은 지역 계획과 안전 기능을 직접 지원할 수 있다. 해당 모델은 표현의 풍부함을 극대화하는 대신 작고, 점진적으로 갱신 가능하며, 제한된 실행시간(bounded execution)을 보장하도록 설계되어야 한다.

느린 경로(slower pathway)는 보다 풍부한 상황 이해(contextual understanding)를 유지할 수 있다. 객체의 기능을 식별하고, 인간 의도(human intent)를 추론하고, 작업 명령(task instruction)을 해석하며, 에피소드 메모리(episodic memory)를 유지하고, 장기 목표(long-term goal)를 평가하거나 미래 행동 시퀀스를 추론할 수 있다. 이러한 과정은 즉각적인 안정화(immediate stabilization)와 상대적으로 느슨하게 결합되어 있기 때문에 더 큰 모델과 더 많은 계산을 사용할 수 있다. 그 결과는 빠른 인지-계획-제어 루프(perception-planning-control loop)를 차단하지 않으면서 비동기적(asynchronously)으로 미래 의사결정에 영향을 줄 수 있다.

점진적 계산(incremental computation)은 월드 모델 지연시간을 감소시키는 데 특히 유용하다. 새로운 센서 프레임이 들어올 때마다 전체 표현을 다시 구축하는 것은 환경 대부분이 변하지 않은 상황에서 계산 자원을 낭비한다. 지속적 잠재 상태(persistent latent state), 순환 갱신(recurrent update), 캐시된 특징(cached feature), 국부 지도 갱신(local map update), 객체 중심 메모리(object-centric memory), 이벤트 기반 처리(event-driven processing)를 사용하면 새로운 관측의 영향을 받는 정보만 수정할 수 있다. 이를 통해 월드 모델링은 반복적인 전체 재구성에서 지속적인 상태 유지(continuous state maintenance) 과정으로 전환된다.

시간적 예측(temporal prediction)은 피할 수 없는 계산 지연을 보상하는 데에도 사용할 수 있다. 월드 모델이 시간 t를 나타내는 관측을 입력받지만 처리가 t+Δt에 완료된다고 가정할 수 있다. 시간 t에 해당하는 상태만 전달하는 대신 해당 상태를 현재 시점이나 예상 행동 시점(expected action time)까지 전파할 수 있다. 그러면 계획 시스템은 시간적으로 보정된 추정값(temporally corrected estimate)을 전달받는다. 따라서 예측은 단순한 추론 능력일 뿐만 아니라 지연시간 보상(latency compensation)을 위한 메커니즘으로도 기능한다.

그러나 예측 범위가 길어질수록 불확실성은 증가한다. 빠르게 생성된 단기 추정(short-horizon estimate)이 계산 비용이 높은 장기 예측보다 더 유용한 경우도 있다. 따라서 아키텍처는 예측 범위, 불확실성 증가(uncertainty growth), 추론시간(inference time), 로봇 속도(robot velocity), 환경 동역학(environmental dynamics), 계획 요구사항을 함께 고려해야 한다. 목표는 가능한 한 먼 미래를 예측하는 것이 아니라, 앞으로 수행할 의사결정에 필요한 시간 범위에서 충분히 신뢰할 수 있는 미래 상태를 제공하는 것이다.

분기되는 미래 시나리오(branching future scenario)는 추론 비용을 크게 증가시킬 수 있다. 로봇이 여러 가능한 행동을 고려할 때 월드 모델은 각 후보 행동(candidate action)에 대해 서로 다른 미래 궤적을 예측할 수 있다. 행동 수, 롤아웃 깊이(rollout depth), 확률적 샘플(stochastic sample), 시뮬레이션 에이전트(simulated agent)의 수를 증가시키면 계산량이 빠르게 증가한다. 따라서 실시간 물리 인공지능(real-time Physical AI)은 선택적 롤아웃(selective rollout), 후보 가지치기(candidate pruning), 계층적 탐색(hierarchical search), 학습된 가치 추정(learned value estimation), 적응형 계산(adaptive computation)을 사용하여 의사결정에 가장 중요한 가능성에 계산 자원을 집중해야 한다.

애니타임 추론(anytime reasoning)은 계산시간이 가변적인 경우 유용한 전략을 제공한다. 시스템은 먼저 실행 가능한 결정이나 대략적인 예측을 빠르게 생성한 후 추가 시간이 남아 있는 동안 이를 개선한다. 마감시간이 가까워지면 추론을 종료하고 현재까지 얻은 최선의 결과를 반환할 수 있다. 이러한 접근방식은 긴 최적화 또는 추론 과정이 완전히 끝날 때까지 사용할 수 있는 출력을 전혀 생성하지 않는 알고리즘보다 물리 시스템에 더 적합하다.

불확실성은 얼마나 많은 추론을 수행할 것인지에도 영향을 주어야 한다. 익숙하고 정적이며 예측 가능한 환경에서는 저비용 월드 모델 갱신만으로 충분할 수 있지만, 익숙하지 않은 교차로, 밀집된 군중, 불안정한 지형, 조작 접촉(manipulation contact), 모호한 객체 상호작용에서는 추가적인 계산이 필요할 수 있다. 적응형 추론(adaptive reasoning)은 항상 최대 계산량을 사용하는 대신 위험과 불확실성에 따라 GPU 연산 자원, 예측 샘플 수, 모델 깊이(model depth), 계획 롤아웃(planning rollout)을 할당할 수 있다.

이러한 특성은 월드 모델 복잡도(world-model complexity)와 사용 가능한 컴퓨팅 자원(compute resource) 사이의 관계를 만든다. 대형 트랜스포머, 멀티모달 인코더(multimodal encoder), 생성형 예측기(generative predictor), 파운데이션 모델(foundation model)은 GPU 메모리, 대역폭, 실행시간을 놓고 인지 시스템과 경쟁할 수 있다. 두 파이프라인이 동일한 가속기(accelerator)를 공유한다면 추론 워크로드가 안전 중요 인지(safety-critical perception)를 지연시킬 수 있다. 따라서 하드웨어-소프트웨어 공동 설계(hardware-software co-design)는 서로 다른 타이밍 요구사항을 가진 워크로드를 위해 스케줄링 우선순위(scheduling priority), 컴퓨팅 파티션(compute partition), 메모리 예산(memory budget), 필요한 경우 별도의 가속기를 정의해야 한다.

엣지(edge), 온프레미스(on-premise), 클라우드(cloud) 배치 역시 추론 지연시간에 영향을 준다. 즉각적인 월드 상태 예측과 안전 관련 추론은 네트워크 통신이 충분히 제한된 응답시간을 보장할 수 없기 때문에 로봇 내부에 유지해야 한다. 반면 계산량이 많은 장기 분석(long-horizon analysis), 플릿 학습(fleet learning), 대규모 모델 추론(large-model inference), 모델 개선(model improvement), 이력 기반 추론(historical reasoning)은 마감시간이 허용하는 경우 온프레미스 또는 클라우드 인프라로 이동할 수 있다. 이는 물리 인공지능 시스템을 위한 보다 광범위한 계층형 컴퓨팅 아키텍처(hierarchical compute architecture)를 따른다.

추론 지연시간은 하나의 평균값이 아니라 분포(distribution)로 측정해야 한다. 입력 복잡도(input complexity), 검출된 객체 수, 생성된 토큰 수, 롤아웃 분기(rollout branch), 메모리 검색(memory retrieval), GPU 경합(GPU contention), 캐시 동작(cache behavior), 열 조건(thermal condition)은 모두 실행시간을 변화시킬 수 있다. 따라서 프로파일링(profiling)은 중앙값(median), 백분위 지연시간(percentile latency), 최악 조건 지연시간(worst-case latency), 마감시간 위반율(deadline-miss rate)을 함께 측정해야 한다. 가변적인 추론 작업이 결정론적 안전 및 제어 경로(deterministic safety and control pathway)를 예측 불가능하게 차단해서는 안 된다.

계산 과부하(computational overload)가 발생하면 추론은 점진적으로 성능을 낮출 수 있어야 한다. 시스템은 예측 범위를 단축하고, 롤아웃 횟수를 줄이며, 후보 행동을 가지치기하고, 더 작은 모델을 사용하며, 캐시된 상태를 재사용하고, 갱신 주기를 낮추거나, 중요하지 않은 의미론적 추론을 일시 중지할 수 있다. 또한 생성형 예측(generative prediction)에서 보다 단순한 동역학 모델(dynamics model)로 전환할 수도 있다. 중요한 국부 월드 상태 추정(local world-state estimation)과 안전 예측(safety prediction)에 우선순위를 부여하여 계산 품질이 일시적으로 감소하더라도 안정적이고 안전한 물리적 동작을 유지해야 한다.

궁극적으로 월드 모델 및 추론 지연시간(world model and reasoning latency)은 인지 결과가 행동을 안내할 수 있는 예측 지능(predictive intelligence)으로 얼마나 빠르게 전환되는지를 결정한다. 더욱 풍부한 내부 모델(internal model)과 깊은 추론(deep reasoning)은 관련된 물리적 상황이 변화하기 전에 결과가 도착할 때에만 가치가 있다. 따라서 물리 인공지능은 빠른 상태 갱신(fast state update), 예측 기반 지연시간 보상(predictive latency compensation), 제한된 추론(bounded reasoning), 적응형 계산, 계층적 시간 경로(hierarchical temporal pathway), 점진적 성능 저하(graceful degradation)를 결합해야 한다. 목표는 모든 순간에 최대한 많은 추론을 수행하는 것이 아니라, 의미 있는 물리적 행동에 사용할 수 있는 시간 안에 적절한 수준의 지능을 제공하는 것이다.

## 06.05. Planning and Action Latency

![](images/image6.png){width="7.268055555555556in" height="7.268055555555556in"}

계획 및 행동 지연시간(planning and action latency)은 물리 인공지능(Physical AI) 시스템이 추정되거나 예측된 월드 상태(world state)를 물리적으로 실행 가능한 행동으로 변환하는 데 필요한 시간이다. 이는 월드 모델링(world modeling)과 저수준 제어(low-level control) 사이의 핵심 영역에 위치하며, 목표, 제약조건(constraint), 예측된 결과, 안전 조건, 로봇 동역학(robot dynamics)을 궤적(trajectory)이나 명령(command)으로 변환한다. 이 과정에서도 물리 환경은 계속 변화하므로 계획 품질은 항상 의사결정 타이밍(decision timing)과 함께 평가되어야 한다.

계획(planning)은 인지(perception)와 월드 모델(world model)이 제공하는 현재 또는 예측 상태의 표현에서 시작된다. 이 표현에는 로봇 자세(robot pose), 속도, 장애물, 자유 공간(free space), 객체 상태, 점유 상태(occupancy), 주행 가능성(traversability), 불확실성(uncertainty), 작업 목표(task goal), 예측된 움직임이 포함될 수 있다. 계획이 시작될 때 이 정보가 이미 오래된 상태라면 계산적으로 최적인 궤적이라도 부적절할 수 있다. 따라서 계획 지연시간은 상위 단계의 정보 연령(information age) 및 예측 정확도(prediction accuracy)와 함께 고려해야 한다.

서로 다른 계획 계층(planning layer)은 자연스럽게 서로 다른 시간 척도(temporal scale)에서 동작한다. 고수준 작업 계획(high-level task planning)은 수초 또는 수분에 걸친 목표, 작업 순서, 행동 모드(behavioral mode)를 결정할 수 있는 반면, 모션 계획(motion planning)은 보다 짧은 범위의 실행 가능한 경로와 궤적을 생성한다. 지역 계획(local planning)과 충돌 회피(collision avoidance)는 수십 밀리초 이내의 갱신이 필요할 수 있다. 행동 생성(action generation)과 저수준 제어는 이보다 더 빠르게 동작한다. 따라서 물리 인공지능 아키텍처는 각 계획 기능이 가져오는 물리적 결과에 따라 서로 다른 마감시간(deadline)을 할당해야 한다.

탐색 기반 계획(search-based planning)은 계획기가 해결책을 선택하기 전에 여러 대안 상태, 행동 또는 궤적을 탐색하기 때문에 상당한 지연시간을 발생시킬 수 있다. 탐색 깊이(search depth), 분기 계수(branching factor), 공간 해상도(spatial resolution), 후보 궤적(candidate trajectory)의 수를 증가시키면 해결책의 품질을 향상시킬 수 있지만 계산량도 빠르게 증가한다. 따라서 실시간 계획(real-time planning)에는 제한된 탐색(bounded search), 휴리스틱 유도(heuristic guidance), 후보 가지치기(candidate pruning), 계층적 분해(hierarchical decomposition), 이전 해결책 재사용을 적용하여 계산 복잡도를 사용 가능한 행동 마감시간과 일치시켜야 한다.

최적화 기반 계획(optimization-based planning)에서도 이와 유사한 절충관계(tradeoff)가 발생한다. 궤적 최적화(trajectory optimization)와 모델 예측 제어(model predictive control)는 만족할 만한 해결책을 얻을 때까지 동역학, 제약조건, 충돌 위험(collision risk), 목적 함수(objective function)를 반복적으로 평가할 수 있다. 최적화 반복 횟수가 증가하면 궤적의 부드러움, 효율성, 안전 여유(safety margin)를 개선할 수 있지만 응답시간도 증가한다. 실제 시스템에서는 반복 횟수 또는 시간 예산(time budget)에 제한을 두고 계획 마감시간이 가까워지면 현재까지 얻어진 최선의 실행 가능한 궤적을 반환하는 경우가 많다.

학습 기반 계획기(learned planner)와 정책(policy)은 상태나 관측을 행동으로 직접 매핑함으로써 일부 탐색 지연을 감소시킬 수 있다. 신경망 정책(neural policy)은 단일 순전파(single forward pass)를 통해 출력을 생성할 수 있으며, 비전-언어-행동 모델(vision-language-action model)은 고수준 또는 토큰화된 행동(tokenized action)을 생성할 수 있다. 그러나 학습 기반 계획이 지연시간 자체를 제거하는 것은 아니다. 대규모 모델 추론, 자기회귀적 행동 생성(autoregressive action generation), 메모리 접근, 멀티모달 처리(multimodal processing), 안전 검증(safety validation), 후속 명령 변환은 여전히 상당하고 가변적인 지연을 발생시킬 수 있다.

예측(prediction)과 계획은 행동이 계획을 시작한 순간이 아니라 미래에 실행되기 때문에 긴밀하게 결합되어 있다. 계획에 Δt 밀리초가 필요하다면 선택된 궤적은 이상적으로 실행이 시작되는 시점에 예상되는 상태에 대해 유효해야 한다. 월드 모델은 로봇과 환경의 상태를 해당 미래 시점까지 전파할 수 있다. 이러한 행동 시점 예측 상태(predicted action-time state)를 기반으로 계획하면 불가피한 계산 지연을 보상하고 물리 세계의 오래된 스냅샷(stale snapshot)에만 기반하여 의사결정을 수행하는 것을 방지할 수 있다.

계획 범위(planning horizon)는 또 다른 주요 지연시간 매개변수이다. 더 긴 계획 범위는 로봇이 멀리 있는 장애물, 미래 상호작용, 에너지 요구량, 작업 결과를 미리 고려할 수 있도록 하지만 평가해야 하는 상태와 행동의 수도 증가시킨다. 짧은 계획 범위는 계산량을 줄이지만 근시안적 행동(myopic behavior)을 발생시킬 수 있다. 계층적 계획(hierarchical planning)은 거친 장거리 계획(coarse long-range planning)과 세밀한 단거리 계획(detailed short-range planning)을 결합하여 이러한 절충관계를 해결할 수 있으며, 즉각적인 물리적 의사결정이 필요한 부분에만 높은 계산 해상도(computational resolution)를 할당할 수 있다.

다중 에이전트 환경(multi-agent environment)은 계획 복잡도를 더욱 증가시킨다. 로봇은 사람, 차량, 다른 로봇 또는 매니퓰레이터의 미래 행동을 예측해야 할 수 있으며, 이들의 미래 행동은 로봇 자신의 행동에도 영향을 받을 수 있다. 각각의 추가적인 상호작용은 여러 가능한 미래를 생성할 수 있다. 모든 시나리오를 실시간으로 완전하게 평가하는 것은 일반적으로 현실적이지 않다. 따라서 계획 지연시간을 제어하려면 선택적 예측(selective prediction), 위험 기반 샘플링(risk-based sampling), 행동 모델(behavior model), 상호작용 인식 휴리스틱(interaction-aware heuristic), 불확실성 인식 가지치기(uncertainty-aware pruning)가 필요하다.

궤적이나 정책 출력이 생성되었다고 해서 행동 선택(action selection)이 완료되는 것은 아니다. 제안된 행동은 실행 가능성 검사(feasibility check), 충돌 검증(collision validation), 액추에이터 제약조건(actuator constraint), 안정성 한계(stability limit), 관절 한계(joint limit), 속도 및 가속도 한계, 힘 제약조건(force constraint), 안전 규칙(safety rule), 런타임 보증 메커니즘(runtime assurance mechanism)을 통과해야 하는 경우가 많다. 이러한 검사는 추가적인 시간을 소비하지만 수학적으로는 유효하더라도 물리적으로 안전하지 않은 명령이 로봇에 전달되는 것을 방지하기 위해 필수적이다.

행동 표현(action representation) 역시 지연시간에 영향을 준다. 계획기는 웨이포인트(waypoint), 궤적, 자세(pose), 속도, 힘, 토크 또는 이산 행동 토큰(discrete action token)을 출력할 수 있다. 고수준 표현(high-level representation)은 후속 제어기가 추가적인 해석, 보간(interpolation), 역기구학(inverse kinematics), 최적화를 수행하도록 요구한다. 저수준 행동(lower-level action)은 변환 단계를 줄일 수 있지만 인공지능 시스템이 로봇 동역학을 보다 직접적으로 추론해야 한다. 따라서 행동 공간 설계(action-space design)는 제어기 아키텍처(controller architecture) 및 타이밍 요구사항과 함께 공동 설계되어야 한다.

행동 지연시간(action latency)은 소프트웨어의 의사결정 완료 시점을 넘어선다. 명령이 선택된 이후에는 미들웨어(middleware), 통신 버스(communication bus), 임베디드 제어기(embedded controller), 모터 드라이브(motor drive), 액추에이터 인터페이스(actuator interface)를 통과해야 한다. 이후 저수준 제어기는 기준값(reference)을 실제 물리적인 힘이나 움직임으로 변환한다. 네트워크 스케줄링, 버스 주기(bus cycle), 명령 버퍼링(command buffering), 제어기 갱신 주기, 모터 전류 동역학(motor current dynamics), 변속기 컴플라이언스(transmission compliance), 관성(inertia), 기계적 응답(mechanical response)이 모두 의사결정 완료부터 실제 물리적 행동까지의 시간에 영향을 준다.

이러한 이유로 계획 주파수(planning frequency)와 행동 지연시간은 별도로 측정해야 한다. 20Hz로 동작하는 계획기는 50밀리초마다 새로운 해결책을 생성하지만 파이프라이닝(pipelining)으로 인해 각각의 해결책을 계산하는 데 실제로는 50밀리초보다 더 긴 시간이 걸릴 수도 있다. 마찬가지로 명령이 빠르게 생성되더라도 다음 제어기 주기(controller cycle)까지 버퍼에 남아 있을 수 있다. 따라서 갱신 주기(update rate), 계산 지연(computation latency), 명령 전송 지연(command transport latency), 액추에이터 응답, 전체 의사결정-투-모션 지연(decision-to-motion delay)은 서로 구별되는 시스템 특성이다.

환경 복잡도(environmental complexity)에 따라 계획시간이 변하는 경우 지터(jitter)는 특히 문제가 된다. 개방된 통로에서는 적은 계산만 필요할 수 있지만 밀집된 군중, 좁은 통로, 조작 접촉(manipulation contact), 다중 로봇 상호작용(multi-robot interaction)은 탐색 또는 최적화 계산량을 크게 증가시킬 수 있다. 실행시간이 예측 불가능해지면 제어 시스템은 새로운 계획이 언제 도착할지 신뢰성 있게 판단할 수 없다. 따라서 계획 알고리즘은 제한된 계산시간을 중심으로 설계하고 평균값뿐만 아니라 백분위 지연시간(percentile latency)과 최악 조건 지연시간(worst-case latency)을 기준으로 측정해야 한다.

애니타임 계획(anytime planning)은 물리 인공지능에 특히 적합하다. 계획기는 먼저 안전하고 실행 가능한 행동을 신속하게 생성한 다음 계산시간이 남아 있는 동안 궤적의 품질을 개선할 수 있다. 마감시간에 도달하면 이상적인 해결책을 기다리는 대신 현재까지 얻은 최선의 해결책을 실행한다. 이러한 접근방식은 환경 조건이나 계산 워크로드가 변하더라도 적시에 행동을 생성할 수 있는 경로를 유지하면서 사용 가능한 컴퓨팅 자원에 따라 계획 품질을 확장할 수 있도록 한다.

이동 지평 계획(receding-horizon planning)은 또 다른 유용한 전략을 제공한다. 로봇은 행동을 시작하기 전에 전체 장기 해결책을 계산하는 대신 제한된 미래 범위를 반복적으로 계획하고, 그중 초기 부분을 실행하고, 갱신된 환경을 다시 관측한 뒤 재계획(replanning)한다. 이를 통해 새로운 정보를 의사결정에 지속적으로 반영할 수 있다. 그러나 재계획 주기는 중요한 환경 변화의 시간 척도보다 짧아야 하며, 새로운 계획이 마감시간을 초과하는 경우에도 시스템은 안전한 대체 행동(safe fallback action)을 유지해야 한다.

적응형 계산(adaptive computation)은 위험과 불확실성에 따라 계획 자원을 할당할 수 있다. 개방된 공간에서 천천히 움직이는 로봇은 더 적은 후보 궤적이나 최적화 반복을 사용할 수 있지만, 고속 이동, 밀집된 장애물, 불확실한 지형, 사람과의 근접 상호작용에서는 보다 깊은 계획을 활성화할 수 있다. 따라서 계산 자원 할당(compute allocation) 자체가 자율성 정책(autonomy policy)의 일부가 될 수 있으며, 모든 조건에서 동일한 계획 워크로드를 유지하는 대신 물리적 문제의 난이도에 맞추어 추론 노력을 조정할 수 있다.

계산 과부하(compute overload)가 발생하더라도 계획은 행동 생성을 중단하는 대신 점진적으로 성능을 낮출 수 있어야 한다. 시스템은 계획 범위를 단축하고, 궤적 샘플 수를 줄이고, 충돌 모델(collision model)을 단순화하며, 최적화 반복 횟수를 줄이거나, 경량 정책(lightweight policy)으로 전환하고, 이전의 유효한 궤적을 재사용하거나 로봇 속도를 낮출 수 있다. 불확실성이 지나치게 높아지면 보수적인 기동(conservative maneuver) 또는 안전 정지(safe stop)로 전환할 수 있다. 최대 수준의 계획 정교함을 유지하는 것보다 안전한 행동을 지속적으로 사용할 수 있도록 하는 것이 더 중요하다.

하드웨어-소프트웨어 공동 설계(hardware-software co-design)는 이러한 타이밍 보장(timing guarantee)을 얼마나 일관되게 달성할 수 있는지를 결정한다. 계획 워크로드는 알고리즘 구조에 따라 CPU, GPU, NPU, 실시간 프로세서(real-time processor) 또는 이들의 조합에서 실행될 수 있다. 월드 모델 예측과 신경망 기반 계획(neural planning)은 가속기의 이점을 활용할 수 있는 반면, 결정론적 안전 검사(deterministic safety check)와 명령 실행은 실시간 제어기에 유지할 수 있다. 이러한 컴퓨팅 분할(compute partitioning)은 가변적인 인공지능 워크로드가 안전 중요 행동 생성(safety-critical action generation)을 차단하는 것을 방지한다.

궁극적으로 계획 및 행동 지연시간(planning and action latency)은 예측 지능(predictive intelligence)이 얼마나 빠르게 실제 물리적 개입(physical intervention)으로 전환되는지를 결정한다. 아무리 좋은 계획이라도 그것을 계산할 때 사용한 가정이 여전히 유효한 동안 로봇에 도달해야만 의미가 있다. 따라서 물리 인공지능은 제한된 계획(bounded planning), 행동 시점 예측(prediction to action time), 계층적 시간 계층(hierarchical temporal layers), 효율적인 행동 표현, 결정론적 안전 검증(deterministic safety validation), 빠른 명령 전달, 점진적 대체 동작(graceful fallback behavior)을 결합해야 한다. 계획의 성공은 이론적으로 가장 좋은 행동을 찾는 것이 아니라, 물리적으로 사용 가능한 시간 안에 충분히 우수하고 안전한 행동을 제공하는 것으로 정의되어야 한다.

## 06.06. Control Loop Timing

![](images/image7.png){width="7.268055555555556in" height="7.268055555555556in"}

제어 루프 타이밍(control loop timing)은 물리 인공지능(Physical AI) 시스템이 자신의 상태를 얼마나 자주 그리고 얼마나 예측 가능하게 측정하고, 제어 응답(control response)을 계산하며, 액추에이터(actuator)를 갱신하는지를 정의한다. 이는 인공지능이 생성한 행동(AI-generated action)과 물리적 동역학(physical dynamics)을 연결하는 가장 기본적인 타이밍 특성 중 하나이다. 인지(perception)와 계획(planning)이 올바른 명령을 생성하더라도 제어 타이밍이 불안정하거나 일관되지 않으면 추종 정확도(tracking accuracy)가 저하되고 움직임이 교란되거나 안전하지 않은 행동이 발생할 수 있다. 따라서 제어 타이밍은 액추에이터 동역학(actuator dynamics), 로봇 기계 시스템(robot mechanics), 작업 요구사항(task requirements), 안전 제약조건(safety constraints)에 따라 설계해야 한다.

제어 루프(control loop)는 센싱-추정-제어-액추에이션(sensing-estimation-control-actuation) 사이클을 반복적으로 수행한다. 인코더(encoder), 전류 센서(current sensor), 힘 센서(force sensor), IMU 또는 기타 피드백 장치(feedback device)가 물리 시스템을 측정한 후, 제어기는 관련 상태를 추정하고 새로운 명령을 계산한다. 이러한 명령은 모터 드라이브(motor drive) 또는 액추에이터를 통해 적용되고 이후 사이클이 다시 반복된다. 연속적인 실행 사이의 시간 간격을 제어 주기(control period)라고 하며, 그 역수는 제어 주파수(control frequency)를 정의한다. 두 값 모두 외란(disturbance)과 명령 변화에 얼마나 빠르게 대응할 수 있는지에 직접적인 영향을 준다.

서로 다른 제어 계층(control layer)은 서로 다른 물리적 동역학을 조절하기 때문에 서로 다른 갱신 주파수(update frequency)를 요구한다. 모터 전류 및 토크 루프(motor current and torque loop)는 수 킬로헤르츠(kilohertz) 이상에서 동작할 수 있고, 속도 루프(velocity loop)는 일반적으로 이보다 느리게 실행되며, 위치 또는 궤적 추종 루프(position or trajectory tracking loop)는 수십에서 수백 헤르츠(hertz) 범위에서 동작할 수 있다. 고수준 인공지능 정책(high-level AI policy), 지역 계획기(local planner), 월드 모델(world model), 의미론적 추론(semantic reasoning)은 이보다 훨씬 낮은 주파수에서 실행될 수 있다. 따라서 물리 인공지능 시스템은 하나의 보편적인 제어 주파수가 아니라 여러 개의 중첩된 계층적 루프(hierarchy of nested loops)를 포함한다.

필요한 제어 주파수는 제어 대상 물리 시스템의 대역폭(bandwidth)과 밀접하게 관련되어 있다. 빠른 전기적·기계적 동역학(electrical and mechanical dynamics)은 상태 변화가 커지기 전에 제어기가 이를 관측할 수 있도록 더 짧은 제어 주기를 요구한다. 느린 플랫폼은 더 긴 시간 간격을 허용할 수 있다. 불필요하게 높은 주파수를 선택하면 컴퓨팅 및 통신 자원을 낭비하지만, 지나치게 낮은 주파수를 선택하면 보정 지연(delayed correction), 진동(oscillation), 낮은 외란 억제 성능(poor disturbance rejection), 불안정성(instability)이 발생할 수 있다.

제어 타이밍은 명목상의 갱신 주파수(nominal update frequency)만으로 결정되지 않는다. 제어기는 다음 갱신이 요구되기 전에 센싱, 상태 처리(state processing), 제어 계산(control computation), 안전 검사(safety check), 명령 전송(command transmission), 액추에이터 인터페이스 연산을 완료해야 한다. 루프가 1밀리초 주기로 설정되어 있다면 해당 사이클과 관련된 모든 시간 중요 계산(time-critical computation)이 이 타이밍 예산(timing budget) 안에 들어가야 한다. 평균 실행시간이 1밀리초보다 짧더라도 일부 사이클이 마감시간(deadline)을 초과한다면 충분하지 않다.

따라서 마감시간 준수(deadline compliance)는 제어의 핵심 요구사항이다. 마감시간을 놓치면 물리 시스템이 계속 변화하는 동안 액추에이터가 이전 명령을 계속 사용할 수 있다. 일부 느린 감독 기능(supervisory function)에서는 간헐적인 지연이 허용될 수 있지만, 안정화(stabilization) 또는 모터 제어 루프에서 반복적이거나 예측할 수 없는 마감시간 위반(deadline miss)이 발생하면 폐루프 동작(closed-loop behavior)이 크게 달라질 수 있다. 따라서 제어 소프트웨어는 평균 계산시간뿐만 아니라 최악 조건 실행시간(worst-case execution time), 마감시간 위반율(deadline-miss rate), 타이밍 분포(timing distribution)를 이용하여 평가해야 한다.

지터(jitter)는 연속적인 제어 루프 실행 타이밍의 변동을 의미한다. 명목상 1kHz로 동작하는 제어기는 이상적으로 1밀리초마다 실행되어야 하지만 운영체제 스케줄링(operating-system scheduling), 인터럽트(interrupt), 메모리 경합(memory contention), 통신 지연(communication delay), 경쟁하는 인공지능 워크로드(competing AI workload)로 인해 개별 실행 시점이 달라질 수 있다. 이러한 변동은 실질적인 샘플링 간격(effective sampling interval)을 변화시키고 고정 주기를 가정하여 설계된 알고리즘의 성능을 저하시킬 수 있다. 따라서 저수준 제어(low-level control)에서는 평균 계산 처리량을 극대화하는 것보다 타이밍 결정성(timing determinism)이 더 중요한 경우가 많다.

물리 인공지능은 결정론적 제어(deterministic control)가 가변적인 인공지능 워크로드(variable AI workload)와 컴퓨팅 및 통신 자원을 공유하는 경우가 많다는 점에서 특별한 문제를 발생시킨다. GPU 추론(GPU inference), 월드 모델 추론(world-model reasoning), 지도 작성(mapping), 로깅(logging), 네트워크 활동(network activity)은 메모리 대역폭, CPU 사이클, 통신 버스, 시스템 자원을 예측하기 어려운 방식으로 사용할 수 있다. 따라서 안전 중요 제어 루프(safety-critical control loop)는 전용 프로세서(dedicated processor), 실시간 스케줄링(real-time scheduling), 우선순위 메커니즘(priority mechanism), 자원 분할(resource partitioning), 별도의 통신 채널 등을 통해 이러한 워크로드로부터 격리되어야 한다.

이러한 특성은 저수준 제어를 MCU, 실시간 CPU(real-time CPU), FPGA, 모터 제어기(motor controller) 또는 기타 결정론적 컴퓨팅 장치(deterministic computing device)에서 실행하고, 인지, 월드 모델링, 계획, 학습 정책(learned policy)은 고성능 인공지능 프로세서에서 실행하는 일반적인 하드웨어-소프트웨어 아키텍처(hardware-software architecture)로 이어진다. 인공지능 계층(AI layer)은 상대적으로 느린 주기로 목표, 궤적, 기준값(reference) 또는 행동을 제공하며, 제어 계층(control layer)은 더 빠른 주기로 안정성과 액추에이터 제어를 지속적으로 유지한다.

인공지능 행동 주파수(AI action frequency)와 액추에이터 제어 주파수(actuator control frequency)의 차이는 명확한 인터페이스 설계를 요구한다. 인공지능 정책이 20Hz에서 새로운 목표 속도(desired velocity)를 생성하는 동안 모터 제어기는 1kHz로 동작할 수 있다. 제어기는 인공지능 갱신 사이에서 단순히 비활성 상태로 머물 수 없다. 여러 개의 빠른 제어 사이클 동안 가장 최근의 기준값을 유지(hold), 보간(interpolate), 필터링(filter), 추종(track)해야 한다. 이러한 시간적 분리(temporal decoupling)를 통해 계산 비용이 높은 지능이 모든 고주파 액추에이터 갱신에 직접 참여하지 않으면서도 로봇의 움직임에 영향을 줄 수 있다.

명령 보간(command interpolation)은 고수준 행동이 상대적으로 낮은 주파수로 도착하는 경우 움직임의 부드러움을 향상시킬 수 있다. 인공지능 추론 사이클마다 목표값이 갑자기 변경되면 속도, 가속도, 토크 또는 힘에 불연속(discontinuity)이 발생할 수 있다. 궤적 생성기(trajectory generator), 필터(filter), 변화율 제한기(rate limiter), 기준값 보간기(reference interpolator)는 낮은 주파수의 인공지능 명령을 부드러운 고주파 기준값으로 변환할 수 있다. 그러나 이러한 메커니즘 역시 동역학과 추가적인 지연을 발생시킬 수 있으므로 전체 제어 설계에 그 동작을 포함해야 한다.

센서 타이밍(sensor timing) 역시 중요하다. 제어 알고리즘은 알려진 물리적 시점에 대응하는 측정값에 의존하기 때문이다. 인코더 측정값, IMU 샘플, 힘 측정값, 액추에이터 피드백(actuator feedback)은 서로 다른 주기나 서로 다른 지연시간으로 도착할 수 있다. 정확한 타임스탬프(timestamp)와 동기화된 클록(synchronized clock)을 사용하면 제어기가 측정값의 정보 연령(measurement age)을 판단할 수 있다. 시간적 일관성(temporal consistency)이 없으면 개별 센서가 모두 정확하더라도 서로 다른 물리적 상태를 나타내는 값이 결합되어 상태 추정 오류(estimation error)가 발생할 수 있다.

통신 버스(communication bus)는 또 다른 타이밍 제약조건을 발생시킨다. CAN, EtherCAT, Ethernet, 직렬 링크(serial link) 등의 인터페이스는 유한한 전송 주기, 중재 동작(arbitration behavior), 버퍼링(buffering), 스케줄링 특성을 갖는다. 명령이나 피드백이 통신 네트워크에서 예측 불가능하게 지연된다면 빠른 제어기를 사용하더라도 결정론적인 물리적 갱신을 달성할 수 없다. 따라서 버스 사용률(bus utilization), 메시지 우선순위(message priority), 동기화, 패킷 스케줄링(packet scheduling), 최악 조건 통신 지연(worst-case communication latency)을 제어 루프 타이밍 예산에 포함해야 한다.

다중 주기 제어 아키텍처(multi-rate control architecture)는 복잡한 로봇에서 특히 유용하다. 빠른 내부 루프(inner loop)는 전류 또는 토크를 제어하고, 중간 루프(middle loop)는 속도 또는 관절 움직임(joint motion)을 제어하며, 외부 루프(outer loop)는 위치, 궤적, 균형(balance), 상호작용 목표(interaction objective)를 추종할 수 있다. 인공지능 계획은 이러한 루프의 외부에서 더욱 느린 시간 척도로 동작할 수 있다. 각 계층은 더 빠른 내부 동역학을 이미 제어된 서브시스템(controlled subsystem)으로 취급할 수 있으므로 모든 알고리즘을 가장 빠른 주파수로 실행하지 않고도 복잡한 물리 인공지능 행동을 구성할 수 있다.

타이밍은 피드백 안정성(feedback stability)에도 영향을 준다. 모든 지연은 폐루프 시스템에 위상 지연(phase lag)을 발생시키기 때문이다. 센서 획득 지연(sensor acquisition delay), 계산 지연(computation delay), 통신 지연, 필터링(filtering), 액추에이터 응답은 종합적으로 변화하는 상태에 대응하는 제어기의 능력을 감소시킨다. 지연시간이 제어 대상 동역학의 시간 척도에 가까워질수록 안정성 여유(stability margin)가 감소할 수 있다. 따라서 제어 루프 타이밍은 단순한 소프트웨어 성능 지표가 아니라 물리적 제어 시스템(physical control system)의 일부로 분석해야 한다.

인공지능이 생성한 명령이 늦게 도착할 경우 저수준 제어기는 명확하게 정의된 대체 동작(fallback behavior)을 가져야 한다. 제한된 시간 동안 가장 최근의 유효한 기준값을 계속 추종하거나, 속도를 낮추거나, 보수적인 제어기(conservative controller)로 전환하거나, 위치를 유지하거나, 안전 정지(safe stop)를 시작할 수 있다. 워치독(watchdog)은 누락되거나 오래된 명령(stale command)을 감지하여 적절한 대응을 활성화할 수 있다. 이를 통해 일시적인 인공지능 과부하, 통신 장애 또는 추론 지연이 즉각적으로 제어되지 않는 액추에이터 동작으로 이어지는 것을 방지한다.

실시간 스케줄링 정책(real-time scheduling policy)은 제어 연산이 필요한 시점에 실행되도록 보장하는 데 도움을 준다. 높은 우선순위의 주기적 작업(high-priority periodic task)은 백그라운드 로깅, 시각화(visualization), 지도 작성, 모델 갱신(model update), 기타 중요도가 낮은 워크로드로부터 격리할 수 있다. 프로세서 어피니티(processor affinity), 사전 할당 메모리(preallocated memory), 제한된 통신 큐(bounded communication queue), 실시간 운영체제(real-time operating system), 인터럽트 관리(interrupt management), 예측하기 어려운 메모리 할당의 회피는 타이밍 변동을 감소시킬 수 있다. 목표는 단순히 빠르게 실행하는 것이 아니라 알려진 시간 범위 내에서 반복 가능하게 실행하는 것이다.

제어 타이밍은 개별 알고리즘의 벤치마크 결과로 추정하는 것이 아니라 실제 배포 시스템(deployed system) 수준에서 측정해야 한다. 엔지니어는 인공지능 워크로드가 동시에 실행되는 상황에서 실제 루프 주기(loop period), 실행시간(execution time), 마감시간 위반, 지터, 센서 정보 연령(sensor age), 통신 지연, 액추에이터 갱신 타이밍(actuator update timing)을 기록해야 한다. 특히 스트레스 테스트(stress testing)가 중요하다. 타이밍 문제는 인지, 추론, 네트워킹, 로깅, 제어가 실제 운용 조건에서 자원을 놓고 경쟁할 때에만 나타나는 경우가 많기 때문이다.

과부하(overload) 상황에서는 중요하지 않은 인공지능 품질을 유지하는 것보다 제어 타이밍을 우선적으로 보호해야 한다. 결정론적 제어가 변경되지 않은 상태로 계속 실행되는 동안 인지 해상도(perception resolution)를 낮추거나, 추론 주파수(reasoning frequency)를 감소시키거나, 지도 작성을 연기하거나, 백그라운드 워크로드를 중지할 수 있다. 충분한 자원을 보장할 수 없다면 로봇의 속도나 작업 복잡도(task complexity)를 줄일 수도 있다. 이러한 우선순위 구조는 계산 지능(computational intelligence)의 성능이 저하되더라도 시스템이 물리적 구현체(physical embodiment)에 대한 안정적인 제어를 잃어서는 안 된다는 물리 인공지능의 기본 원칙을 반영한다.

궁극적으로 제어 루프 타이밍(control loop timing)은 디지털 의사결정(digital decision)이 안정적이고 예측 가능하며 안전한 물리적 움직임으로 변환될 수 있는지를 결정한다. 높은 주파수만으로는 충분하지 않으며, 제어 루프는 제한된 지연시간(bounded latency), 낮은 지터(low jitter), 동기화된 피드백(synchronized feedback), 신뢰할 수 있는 통신(reliable communication), 적절한 대체 동작을 갖추고 실행되어야 한다. 따라서 물리 인공지능은 빠르고 결정론적인 제어와 상대적으로 느리고 가변적인 지연시간을 가진 지능(variable-latency intelligence)을 분리하면서, 신중하게 설계된 시간적 인터페이스(temporal interface)를 통해 이들을 조정하는 다중 주기 아키텍처(multi-rate architecture)를 요구한다.

## 06.07. Hard vs Soft Real Time

![](images/image8.png){width="7.268055555555556in" height="7.268055555555556in"}

하드 실시간(hard real-time)과 소프트 실시간(soft real-time)은 물리 인공지능(Physical AI) 시스템에서 서로 다른 수준의 타이밍 엄격성(timing strictness)을 설명한다. 두 방식 모두 정의된 시간 제약조건(time constraint) 안에서 유용한 계산 결과를 생성해야 하지만, 마감시간(deadline)을 놓쳤을 때 발생하는 결과에서 차이가 있다. 하드 실시간 동작에서는 물리적 안전이나 안정성이 정확한 타이밍에 의존할 수 있기 때문에 늦게 도착한 결과 자체가 시스템 실패(system failure)로 간주될 수 있다. 소프트 실시간 동작에서는 간헐적인 지연을 허용할 수 있지만 성능, 정확도, 반응성(responsiveness), 사용자 경험(user experience)이 저하될 수 있다.

실시간 마감시간(real-time deadline)은 단순히 빠른 계산을 위한 목표 시간이 아니다. 이는 결과가 의도된 물리적 기능에 대해 유효성을 유지할 수 있는 가장 늦은 시점을 정의한다. 모터 제어기(motor controller)가 1밀리초마다 토크를 갱신해야 한다면 해당 마감시간 이후에 도착한 명령은 더 이상 예상된 제어 상태와 일치하지 않을 수 있다. 반면 의미론적 지도 갱신(semantic map update)이 수백 밀리초 지연되더라도 여전히 유용할 수 있다. 따라서 실시간 분류(real-time classification)는 계산 속도만이 아니라 물리적 결과(physical consequence)를 기준으로 결정해야 한다.

하드 실시간 시스템(hard real-time system)은 지정된 운용 조건에서 마감시간을 예측 가능하게 충족해야 한다. 설계에서는 결정론적 실행(deterministic execution), 제한된 최악 조건 지연시간(bounded worst-case latency), 통제된 스케줄링(controlled scheduling), 예측 가능한 통신(predictable communication), 관련 없는 워크로드의 제한된 간섭을 중요하게 다룬다. 드물게 발생하는 타이밍 위반조차 허용할 수 없기 때문에 평균 성능만으로는 충분하지 않다. 따라서 하드 실시간 보장을 설정할 때 최악 조건 실행시간(worst-case execution time), 인터럽트 동작(interrupt behavior), 메모리 접근, 통신 지연, 작업 우선순위(task priority), 하드웨어 응답을 고려해야 한다.

저수준 액추에이터 제어(low-level actuator regulation)는 하드 실시간 요구사항에 가까운 대표적인 기능이다. 모터 전류, 토크, 안정화(stabilization), 균형(balance), 고속 모션 제어(high-speed motion-control) 루프는 수백 헤르츠에서 수 킬로헤르츠(kilohertz)의 주파수로 실행되어야 할 수 있다. 여러 개의 연속적인 제어 주기를 놓치면 폐루프 동역학(closed-loop dynamics)이 변하고 안정성 여유(stability margin)가 감소할 수 있다. 따라서 이러한 기능은 일반적으로 가변 지연시간 인공지능 추론(variable-latency AI inference)에 직접 의존하지 않고 MCU, 실시간 프로세서(real-time processor), FPGA, 모터 드라이브(motor drive), 전용 제어기(dedicated controller)에 배치된다.

비상 및 안전 기능(emergency and safety function) 역시 엄격한 타이밍을 요구할 수 있다. 충돌 보호(collision protection), 비상 정지(emergency stopping), 액추에이터 한계 적용(actuator limit enforcement), 워치독 감시(watchdog supervision), 안전 인터록(safety interlock)은 위험한 물리적 상태가 복구 불가능한 수준으로 발전하기 전에 반응해야 한다. 관련 마감시간은 로봇 속도, 질량, 정지 거리(stopping distance), 액추에이터 성능, 센싱 범위(sensing range), 사람과의 근접성에 따라 달라진다. 따라서 하드 실시간 요구사항은 임의로 설정하기보다 위험 분석(hazard analysis)과 물리적 동역학을 기반으로 도출해야 한다.

소프트 실시간 시스템(soft real-time system)에도 타이밍 목표가 존재하지만 마감시간 위반이 즉각적으로 치명적인 실패를 의미하지는 않는다. 객체 분류(object classification), 의미론적 해석(semantic interpretation), 전역 지도 갱신(global map update), 작업 추천(task recommendation), 장기 예측(long-horizon prediction)이 지연되면 로봇이 안전하게 계속 동작하는 동안 자율성 품질(autonomy quality)이 일시적으로 감소할 수 있다. 따라서 모든 계산을 절대적인 시간 한계 내에 완료하도록 요구하기보다는 평균 지연시간, 백분위 지연시간(percentile latency), 정보 최신성(freshness), 처리량(throughput), 품질 저하(quality degradation)를 이용하여 성능을 측정할 수 있다.

많은 인공지능 워크로드(AI workload)는 모델 아키텍처(model architecture), 입력 복잡도(input complexity), GPU 스케줄링, 메모리 경합(memory contention), 토큰 생성(token generation), 동시 워크로드에 따라 실행시간이 달라지기 때문에 자연스럽게 소프트 실시간 영역에 속한다. 객체 검출(object detection), 분할(segmentation), 월드 모델 예측(world-model prediction), 멀티모달 추론(multimodal reasoning), 비전-언어 처리(vision-language processing), 전역 계획(global planning)은 상당한 타이밍 변동을 나타낼 수 있다. 이러한 모든 계산을 하드 실시간으로 처리하려 하면 과도한 하드웨어 비용과 불필요하게 제한적인 시스템 설계가 발생할 수 있다.

그러나 특정 기능을 단순히 알고리즘 이름만으로 하드 또는 소프트 실시간으로 분류할 수는 없다. 동일한 인지 알고리즘(perception algorithm)이라도 출력 결과가 어떻게 사용되는지에 따라 타이밍 중요도(timing criticality)가 달라질 수 있다. 비상 제동(emergency braking)을 직접 제어하는 장애물 검출기(obstacle detector)는 엄격하게 제한된 응답시간을 요구할 수 있지만, 동일한 검출기가 지도에 객체를 표시하는 용도로 사용된다면 더 큰 지연을 허용할 수 있다. 따라서 실시간 분류는 단순한 소프트웨어 모듈이 아니라 전체 기능과 그 물리적 결과를 기준으로 결정해야 한다.

하드 실시간과 소프트 실시간 사이에서 실제 시스템은 확정 실시간(firm real-time) 특성을 가진 기능을 포함하는 경우도 많다. 결과는 마감시간 이전에는 상당한 가치를 갖지만 마감시간 이후에는 즉각적인 시스템 실패를 발생시키지 않더라도 더 이상 쓸모가 없어질 수 있다. 예를 들어 로봇이 이미 관련 의사결정 지점을 지나간 이후 계산된 지역 궤적(local trajectory)은 의미가 없다. 이러한 중간 범주는 물리 인공지능의 타이밍이 두 개의 이산적인 범주로 완벽하게 구분되는 것이 아니라 연속적인 스펙트럼(spectrum)으로 존재한다는 점을 보여준다.

중요한 아키텍처 원칙은 소프트 실시간 인공지능 워크로드가 하드 실시간 제어를 차단하지 않도록 하는 것이다. 대규모 GPU 추론, 월드 모델 롤아웃(world-model rollout), 로깅(logging), 시각화(visualization), 지도 작성(mapping), 언어 추론(language reasoning)이 액추에이터 안정화(actuator stabilization)나 안전 모니터링(safety monitoring)을 지연시켜서는 안 된다. 하드웨어-소프트웨어 공동 설계(hardware-software co-design)는 별도의 프로세서, 실시간 운영체제(real-time operating system), 우선순위 스케줄링(priority scheduling), 프로세서 어피니티(processor affinity), 예약된 통신 대역폭(reserved communication bandwidth), 메모리 분할(memory partitioning), 전용 안전 제어기(dedicated safety controller)를 통해 중요 작업을 격리할 수 있다.

이러한 분리는 계층적 컴퓨팅 아키텍처(hierarchical compute architecture)를 형성한다. 빠르고 결정론적인 루프(deterministic loop)는 센서와 액추에이터 가까이에 유지되고, 점점 더 복잡하고 가변적인 계산은 상위 계층에서 실행된다. 모터 제어기는 킬로헤르츠 수준으로 동작하고, 모션 제어(motion control)는 수백 헤르츠, 인지 및 지역 계획(local planning)은 수십 헤르츠, 의미론적 또는 장기 추론(long-horizon reasoning)은 이보다 더 낮은 주파수로 동작할 수 있다. 각 계층은 전체 시스템에서 동일한 타이밍 동작을 가정하는 대신 명시적으로 정의된 시간적 인터페이스(temporal interface)를 통해 통신한다.

하드 실시간 영역과 소프트 실시간 영역 사이의 인터페이스는 신중하게 설계해야 한다. 느린 인공지능 모듈은 목표 속도(desired velocity), 궤적(trajectory), 목표 자세(target pose), 행동 모드(behavioral mode) 또는 기타 기준값(reference)을 더 빠른 결정론적 제어기에 제공할 수 있다. 인공지능 갱신 사이에도 제어기는 가장 최근의 유효한 기준값을 계속 추종한다. 보간(interpolation), 필터링(filtering), 궤적 생성(trajectory generation), 변화율 제한(rate limiting)을 통해 불규칙한 고수준 명령을 부드러운 고주파 제어 기준값으로 변환하면서 인공지능의 타이밍 변동이 액추에이터에 직접 전달되는 것을 방지할 수 있다.

소프트 실시간 출력이 보다 중요한 기능으로 전달될 때는 정보 최신성 한계(freshness limit)가 필수적이다. 궤적, 검출된 장애물 상태, 인공지능 명령에는 타임스탬프(timestamp) 또는 유효성 정보(validity information)가 포함되어 후속 제어기가 해당 정보를 여전히 안전하게 사용할 수 있는지 판단할 수 있어야 한다. 논리적으로 정확한 결과라도 시간적으로 오래되었다면 물리적으로는 잘못된 정보가 될 수 있다. 따라서 시스템에는 최대 정보 연령 한계(maximum-age limit), 타임아웃 메커니즘(timeout mechanism), 시퀀스 번호(sequence number), 동기화된 클록(synchronized clock), 오래된 정보(stale information)를 거부하기 위한 명시적 정책이 필요하다.

워치독(watchdog)은 서로 다른 타이밍 영역 사이의 또 다른 보호 경계를 제공한다. 결정론적 제어기는 예상되는 명령이나 상태 갱신이 허용된 시간 안에 도착하는지 감시할 수 있다. 인공지능 계층이 과부하되거나 충돌(crash)하거나 통신이 끊기면 워치독이 미리 정의된 동작을 활성화할 수 있다. 응용에 따라 로봇은 가장 최근의 유효한 명령을 짧은 시간 동안 유지하거나, 속도를 줄이고, 더 단순한 제어기로 전환하거나, 위치를 유지하거나, 성능 저하 운용 모드(degraded operating mode)로 진입하거나, 안전 정지(safe stop)를 수행할 수 있다.

점진적 성능 저하(graceful degradation)는 소프트 실시간 지능에서 특히 중요하다. 컴퓨팅 자원이 제한되면 시스템은 카메라 해상도를 낮추고, 인지 주파수(perception frequency)를 줄이고, 예측 범위(prediction horizon)를 단축하고, 계획 샘플 수(planning sample)를 감소시키거나, 의미론적 추론을 일시 중지하거나, 더 작은 모델을 선택할 수 있다. 이러한 변경은 인공지능 품질을 감소시키지만 필수 기능의 타이밍을 유지한다. 반면 하드 실시간 작업은 시스템이 정의된 운용 범위(operating envelope) 안에 있는 동안 예약된 자원과 결정론적 실행을 유지해야 한다.

네트워크 기반 계산(networked computation)은 이러한 구분을 더욱 명확하게 만든다. 클라우드 또는 원격 서버 처리(remote-server processing)는 통신 지연, 혼잡(congestion), 패킷 손실(packet loss), 라우팅 변화(routing change), 일시적인 연결 단절을 엄격하게 제한하기 어렵기 때문에 일반적으로 하드 실시간 보장을 제공할 수 없다. 따라서 안전 중요 제어(safety-critical control)는 로컬에 유지해야 한다. 온프레미스(on-premise) 또는 클라우드 자원은 플릿 분석(fleet analysis), 모델 학습(model training), 장기 추론, 이력 기반 최적화(historical optimization)와 같이 타이밍 요구사항이 상대적으로 덜 엄격한 기능에 더 적합하다.

하드 실시간 동작을 시험하려면 정상적인 운용 조건에서 평균 지연시간만 측정해서는 충분하지 않다. 시스템은 예상되는 최대 CPU 부하, GPU 활동, 네트워크 트래픽, 센서 대역폭(sensor bandwidth), 로깅, 메모리 압력(memory pressure), 열 조건(thermal condition)에서 스트레스 테스트(stress test)를 수행해야 한다. 엔지니어는 이러한 조건에서도 중요한 마감시간이 계속 충족되는지 확인해야 한다. 최악 조건 실행시간, 최대 지터(maximum jitter), 통신 지연 한계(communication bound), 마감시간 위반율(deadline-miss rate), 장애 복구 동작(failure recovery behavior)이 핵심 검증 지표이다.

소프트 실시간 검증(soft real-time validation)은 보다 광범위한 서비스 품질(quality of service) 관점을 사용한다. 엔지니어는 중앙값 및 백분위 지연시간, 처리량, 정보 연령(information age), 드롭된 프레임(dropped frame), 계획 갱신 주파수(planning update frequency), 예측 최신성(prediction freshness), 사용자가 체감하는 반응성 등을 측정할 수 있다. 시스템이 안전한 상태를 유지하고 전체 성능이 요구사항 안에 있다면 간헐적인 마감시간 위반은 허용될 수 있다. 중요한 설계 문제는 기능을 단순화하거나 건너뛰거나 대체하기 전에 어느 정도의 타이밍 성능 저하를 허용할 것인지 결정하는 것이다.

하드 실시간과 소프트 실시간 사이의 경계는 운용 조건에 따라 동적으로 변화할 수도 있다. 로봇이 정지해 있을 때 상대적으로 중요하지 않은 인지 기능도 사람이나 장애물 주변에서 로봇이 빠르게 이동할 때는 매우 높은 시간 민감도(time sensitivity)를 가질 수 있다. 시스템은 갱신 주파수를 높이고, 추가 컴퓨팅 자원을 할당하고, 최대 속도를 낮추거나, 전용 안전 경로(dedicated safety pathway)를 활성화하여 대응할 수 있다. 따라서 타이밍 중요도는 위험(risk), 속도(velocity), 불확실성, 환경, 운용 모드(operating mode)에 따라 달라질 수 있다.

따라서 물리 인공지능은 전체 자율주행 및 자율행동 스택(autonomy stack)을 동일하게 취급하기보다 타이밍 중요도 계층(timing-criticality hierarchy)을 사용하는 것이 유리하다. 안전과 안정화에는 가장 강력한 결정론적 보장(deterministic guarantee)을 제공하고, 지역 움직임 및 충돌 관련 기능에는 엄격하게 제한된 응답시간 목표를 적용하며, 인지와 예측 계획(predictive planning)은 실시간 품질 제약조건(real-time quality constraint) 아래에서 동작하도록 할 수 있다. 의미론적 추론이나 플릿 지능(fleet intelligence)은 더 큰 타이밍 변동을 허용할 수 있다. 이러한 계층 구조를 통해 물리적 결과의 중요성에 따라 계산 자원을 할당할 수 있다.

궁극적으로 하드 실시간(hard real-time)과 소프트 실시간(soft real-time)의 차이는 계산 결과가 늦게 도착했을 때 어떤 일이 발생하는가에 있다. 하드 실시간 기능은 마감시간을 놓치면 안전, 안정성 또는 시스템 정확성(system correctness)을 위반할 수 있기 때문에 반드시 마감시간을 충족해야 한다. 소프트 실시간 기능은 지연되면 품질이 저하되지만 정의된 범위 안에서는 이를 허용할 수 있기 때문에 가능한 한 마감시간을 충족해야 한다. 성공적인 물리 인공지능은 이러한 타이밍 영역을 분리하고, 가변적인 인공지능 워크로드로부터 결정론적 제어를 보호하며, 오래된 정보를 거부하고, 상위 수준 지능이 제시간에 응답하지 못하는 경우에도 안전한 대체 동작(safe fallback behavior)을 제공해야 한다.

## 06.08. End to End Latency Budget

![](images/image9.png){width="7.268055555555556in" height="7.268055555555556in"}

종단간 지연시간 예산(end-to-end latency budget)은 물리 인공지능(Physical AI) 시스템이 물리적 사건을 감지한 시점부터 필요한 물리적 반응을 생성할 때까지 사용할 수 있는 최대 시간을 정의한다. 인지(perception), 추론(reasoning), 계획(planning), 제어(control)를 개별적으로 최적화하는 대신 전체 센싱-투-액션(sensing-to-action) 체인을 하나의 타이밍 시스템(timing system)으로 취급한다. 이 예산은 정보나 의사결정이 너무 오래되어 안전성, 정확성 또는 유용성을 잃기 전에 로봇이 허용할 수 있는 지연시간을 설정한다.

지연시간 예산(latency budget)은 사용 가능한 프로세서만을 기준으로 결정하는 것이 아니라 물리적 작업(physical task)에서 도출해야 한다. 로봇 속도, 액추에이터 동역학(actuator dynamics), 장애물 거리, 제동 능력(braking capability), 상호작용 속도, 환경 불확실성(environmental uncertainty), 안전 여유(safety margin)가 최대 허용 반응시간(maximum acceptable reaction time)을 결정한다. 느리게 움직이는 매니퓰레이터(manipulator)는 민첩한 로봇이나 고속 차량보다 더 큰 예산을 허용할 수 있다. 따라서 먼저 물리적 마감시간(physical deadline)을 설정하고 이후 이를 만족하도록 컴퓨팅 자원을 설계해야 한다.

전체 지연시간 경로(latency path)는 센서 획득(sensor acquisition)에서 시작하여 데이터 전송, 버퍼링(buffering), 전처리(preprocessing), 인지, 상태 추정(state estimation), 월드 모델링(world modeling), 추론, 계획, 명령 생성(command generation), 저수준 제어(low-level control), 액추에이션(actuation), 기계적 응답(mechanical response)으로 이어진다. 각 단계는 사용 가능한 반응시간의 일부를 소비한다. 따라서 대표적인 아키텍처는 노출(exposure) 또는 스캐닝(scanning)부터 인공지능 계산과 제어를 거쳐 실제 물리적 움직임이 변화하는 순간까지의 전체 과정에 시간 예산을 할당한다.

지연시간 예산은 개념적으로 각 단계별 지연시간(stage-level delay)의 합으로 표현할 수 있다. 전체 허용 응답시간이 100밀리초라면 그 시간의 일부를 센싱, 인지, 월드 모델 추론(world-model reasoning), 계획, 명령 및 제어(command and control), 액추에이션에 할당할 수 있다. 하나의 예시 아키텍처에서는 100밀리초를 이러한 기능들에 분배하여 설명할 수 있지만, 정확한 할당은 시스템에 따라 달라지며 보편적인 타이밍 규격(universal timing specification)을 의미하지 않는다.

단계별 예산(stage budget)은 각각의 모듈이 독립적으로 사용할 수 있는 최대 시간이거나 모든 시간을 완전히 소비해도 된다는 의미로 해석해서는 안 된다. 이는 하나의 공유된 시스템 예산(shared system budget) 안에서 적용되는 공학적 제약조건(engineering constraint)이다. 인지에 예상보다 많은 시간이 필요하면 전체 마감시간이 변경되지 않는 한 추론과 계획에 사용할 수 있는 시간은 줄어든다. 반대로 한 단계를 최적화하면 다른 단계에 타이밍 여유(timing margin)를 제공할 수 있다. 따라서 종단간 예산 관리는 전체 폐루프(closed loop)에 미치는 영향을 기준으로 아키텍처 결정을 평가하도록 한다.

센싱 지연시간(sensing latency)에는 노출, 스캐닝, 샘플링(sampling), 센서 내부 처리(sensor-internal processing), 타임스탬프 생성(timestamp generation), 장치로부터의 데이터 전송이 포함된다. 카메라 프레임은 순간적으로 생성되는 것이 아니며 회전식 또는 스캐닝 방식의 LiDAR는 일정 시간 동안 누적된 측정값을 표현한다. 이러한 지연은 인공지능 추론이 시작되기도 전에 지연시간 예산의 일부를 소비한다. 따라서 시스템은 데이터가 소프트웨어에 도착한 시점이 아니라 각각의 관측이 실제로 나타내는 물리적 시점을 알아야 하므로 정확한 타임스탬프가 필수적이다.

데이터 이동(data movement)에도 명시적인 예산을 할당해야 한다. 센서 인터페이스(sensor interface), Ethernet 스위치, PCIe 전송, 직접 메모리 접근(DMA), 미들웨어(middleware), CPU-GPU 전송, 프로세스 간 통신(inter-process communication), 큐(queue)는 상당한 예산을 소비할 수 있다. 고대역폭 멀티카메라 및 LiDAR 시스템에서는 이러한 영향이 특히 중요하다. 처리량은 가장 취약한 연결 구간에 의해 제한될 수 있으며 버퍼링과 동기화가 보이지 않게 지연시간을 증가시킬 수 있으므로 전체 데이터 경로를 분석해야 한다.

인지는 인공지능 지연시간 예산(AI latency budget)의 상당 부분을 소비하는 경우가 많다. 영상 처리, 특징 인코딩(feature encoding), 검출(detection), 분할(segmentation), 점유 추정(occupancy estimation), 추적(tracking), 융합(fusion), 위치 추정(localization)은 순차적으로 또는 동시에 실행될 수 있다. 모델 크기나 센서 해상도를 증가시키면 정확도를 향상시킬 수 있지만 사용 가능한 시간을 더 많이 소비한다. 따라서 모델 선택에서는 예측, 계획, 안전 검증(safety validation), 행동에 남겨야 하는 종단간 타이밍 여유를 함께 고려해야 한다.

월드 모델 및 추론 계산(world-model and reasoning computation)은 또 다른 예산 할당 영역이다. 미래 상태 예측은 피할 수 없는 지연을 보상할 수 있지만, 더 긴 예측 범위(prediction horizon), 더 큰 트랜스포머(transformer), 멀티모달 추론(multimodal reasoning), 여러 개의 롤아웃 분기(rollout branch)는 계산량을 증가시킨다. 아키텍처는 정교한 추론이 물리적 반응에 필요한 마감시간을 소비하지 않으면서 의사결정에 필요한 예측을 생성할 수 있도록 충분한 시간을 할당해야 한다. 따라서 빠른 국부 예측(local prediction)과 느린 고수준 추론(high-level reasoning)은 서로 다른 시간적 경로(temporal pathway)를 사용할 수 있다.

계획은 상태 정보가 생성된 이후 남아 있는 타이밍 여유 안에서 동작해야 한다. 탐색 깊이(search depth), 궤적 샘플(trajectory sample), 최적화 반복(optimization iteration), 충돌 검사(collision check), 행동 후보(action candidate)는 계획 마감시간(planning deadline)에 의해 제한되어야 한다. 계획 알고리즘은 시간 예산, 조기 종료(early termination), 후보 가지치기(candidate pruning), 애니타임 계산(anytime computation)을 사용하여 사용 가능한 시간이 끝나기 전에 안전하고 실행 가능한 행동을 확보할 수 있다. 이론적으로 가장 좋은 궤적이라도 관련 물리적 상황이 이미 변화한 이후에 도착한다면 가치가 거의 없다.

명령 생성과 저수준 제어에는 상대적으로 작지만 더욱 결정론적인 타이밍 할당(deterministic timing allocation)이 필요하다. 고수준 행동은 실행 가능한 기준값(reference)으로 변환되고, 제약조건에 대한 검사를 거치며, 임베디드 제어기(embedded controller)로 전송되어 액추에이터 명령으로 변환되어야 한다. 이러한 연산은 인지나 추론보다 계산량이 적을 수 있지만 마감시간은 더 엄격한 경우가 많다. 따라서 전체 아키텍처는 느리고 가변적인 인공지능 계산과 빠르고 결정론적인 제어 영역(deterministic control domain)을 분리할 필요가 있다.

액추에이션 및 기계적 응답은 소프트웨어 지연시간의 외부 요소로 취급하지 않고 종단간 예산 안에 포함해야 한다. 모터 드라이브에는 갱신 주기(update period)가 존재하고, 모터가 토크를 발생시키는 데에는 유한한 시간이 필요하며, 변속기와 구조물에는 컴플라이언스(compliance)가 존재하고, 움직이는 물체는 관성(inertia)을 가진다. 따라서 의미 있는 종단점(endpoint)은 단순히 소프트웨어가 명령을 전송한 순간이 아니라 실제 물리적 반응이 발생한 순간이다. 종단간 타이밍은 계산 지연을 물리적 구현(embodiment)과 직접 연결한다.

파이프라인 중첩(pipeline overlap)은 단순한 지연시간 합산을 복잡하게 만든다. 센서 획득은 전처리와 중첩될 수 있고, 여러 카메라 스트림은 동시에 실행될 수 있으며, GPU 추론은 CPU 작업과 병렬로 수행될 수 있고, 고수준 계획이 실행되는 동안에도 저수준 제어기는 계속 동작할 수 있다. 병렬 처리(parallelism)는 실질적인 지연시간을 줄일 수 있지만 컴퓨팅, 메모리, 통신 자원의 경합(contention)을 발생시킬 수도 있다. 따라서 유효한 지연시간 예산은 개별 벤치마크 시간을 단순히 더하는 것이 아니라 실제 실행 의존성(execution dependency)과 임계 경로(critical path)를 따라야 한다.

정보 연령(information age)은 지연시간 예산을 이해하는 또 다른 방법이다. 20밀리초 만에 완료된 인지 출력도 노출, 전송, 동기화, 큐잉(queueing)으로 인해 훨씬 이전에 획득된 관측을 나타낼 수 있다. 중요한 것은 해당 정보가 최종적으로 물리적 행동을 생성하는 데 사용될 때 얼마나 오래된 정보인가이다. 따라서 센서 타임스탬프, 큐 깊이(queue depth), 드롭된 프레임(dropped frame), 처리 지연(processing delay), 마감시간을 모니터링하여 로봇이 현재의 세계를 기반으로 행동하는지 아니면 오래된 세계를 기반으로 행동하는지 파악해야 한다.

타이밍 여유는 이론적인 마감시간의 100%를 정상 계산에 할당하는 대신 의도적으로 확보해야 한다. 운영체제 스케줄링(operating-system scheduling), 메모리 경합(memory contention), GPU 간섭(GPU interference), 통신 변동성, 캐시 효과(cache effect), 열 스로틀링(thermal throttling), 장면 의존적 워크로드(scene-dependent workload)는 예상하지 못한 지연시간 증가를 발생시킬 수 있다. 따라서 예산에는 변동성과 불확실성을 위한 예비 여유(reserve margin)가 필요하다. 평균 실행시간만을 기준으로 설계하면 벤치마크에서는 빠르게 보이지만 실제 운용에서는 반복적으로 마감시간을 위반하는 시스템이 될 수 있다.

종단간 지연시간은 상수가 아니라 분포(distribution)이기 때문에 지터(jitter)를 명시적으로 포함해야 한다. 평균 지연시간이 동일한 두 시스템이라도 한 시스템은 타이밍이 엄격하게 제한되어 있는 반면 다른 시스템은 간헐적으로 큰 지연을 발생시킬 수 있다. 따라서 평균 지연시간과 함께 백분위 지연시간(percentile latency), 최악 조건 동작(worst-case behavior), 최대 정보 연령(maximum information age), 마감시간 위반 확률(deadline-miss probability)을 평가해야 한다. 실시간 성능은 속도뿐만 아니라 예측 가능성(predictability)에도 의존한다.

큐잉은 개별 알고리즘의 실행속도가 느려지지 않더라도 전체 지연시간 예산을 무너뜨릴 수 있기 때문에 특별한 주의가 필요하다. 입력 센서 데이터가 처리 속도보다 빠르게 도착하면 큐가 누적되고 정보 연령이 지속적으로 증가한다. 제한된 버퍼(bounded buffer), 최신 프레임 처리(latest-frame processing), 제어된 프레임 드롭(controlled frame dropping), 워크로드 우선순위화(workload prioritization), 백프레셔(backpressure)를 사용하면 이러한 상황을 방지할 수 있다. 물리 인공지능에서는 모든 과거 관측을 보존하는 것보다 가장 최신의 관련 상태를 처리하는 것이 더 중요한 경우가 많다.

지연시간 예산은 공유 자원(shared resource) 사이의 상호작용도 고려해야 한다. 인지, 월드 모델, 계획, 지도 작성(mapping), 로깅(logging), 통신은 GPU 사이클, CPU 코어, 메모리 대역폭, 네트워크 용량을 놓고 경쟁할 수 있다. 단독 실행에서는 예산을 만족하는 모듈도 다른 워크로드와 동시에 실행되면 이를 위반할 수 있다. 따라서 하드웨어-소프트웨어 공동 설계(hardware-software co-design)는 실제 시스템 부하에서 동시 실행(concurrency), 자원 예약(resource reservation), 우선순위 스케줄링(priority scheduling), 메모리 지역성(memory locality), 컴퓨팅 분할(compute partitioning)을 평가해야 한다.

서로 다른 타이밍 중요도 등급(timing-criticality class)에는 서로 다른 수준의 보호가 제공되어야 한다. 하드 실시간(hard real-time) 안정화와 안전 기능에는 강력하게 제한된 자원이 필요하고, 인지와 지역 계획(local planning)은 보다 엄격한 소프트 또는 확정 실시간(soft or firm real-time) 목표 아래에서 동작할 수 있다. 의미론적 추론(semantic reasoning), 지도 작성, 플릿 지능(fleet intelligence)은 더 큰 변동성을 허용할 수 있다. 따라서 종단간 예산은 모든 구성요소에 동일한 타이밍 보장을 적용하는 것이 아니라 각각의 물리적 결과에 적합한 보장을 제공한다.

적응형 계산(adaptive computation)은 워크로드가 변화할 때 전체 예산을 유지하는 데 도움을 줄 수 있다. 시스템은 영상 해상도를 낮추고, 오래된 프레임을 건너뛰며, 더 작은 모델을 선택하고, 예측 범위를 단축하며, 롤아웃 횟수를 줄이고, 계획을 단순화하거나, 백그라운드 지도 작성을 연기할 수 있다. 타이밍 여유가 계속 감소한다면 로봇 속도를 낮춰 물리적 반응시간 요구사항 자체를 완화할 수도 있다. 따라서 계산 적응(computational adaptation)과 물리적 행동(physical behavior)은 안전성을 유지하기 위해 상호 협력할 수 있다.

검증(validation)은 현실적이고 스트레스가 가해진 조건에서 전체 배포 파이프라인(deployed pipeline)을 측정해야 한다. 엔지니어는 센서 획득, 인공지능 단계의 시작과 종료, 계획 완료, 명령 전송, 제어기 수신, 액추에이터 갱신, 관측 가능한 물리적 반응에 각각 타임스탬프를 기록해야 한다. CPU, GPU, 네트워크, 로깅, 메모리, 센서 워크로드가 동시에 실행되는 동안 측정을 반복해야 한다. 목적은 실제 임계 경로를 식별하고 타이밍 예산이 실제로 어느 부분에서 소비되는지를 확인하는 것이다.

궁극적으로 종단간 지연시간 예산(end-to-end latency budget)은 실시간 성능(real-time performance)을 막연한 목표가 아니라 시스템 수준의 공학적 제약조건(system-level engineering constraint)으로 변환한다. 이는 센싱, 데이터 이동, 인지, 월드 모델링, 추론, 계획, 제어, 통신, 액추에이션, 물리적 동역학을 하나의 공유된 마감시간으로 연결한다. 성공적인 물리 인공지능은 개별 모듈을 단순히 빠르게 만드는 것이 아니라 전체 폐루프에 걸쳐 타이밍을 할당하고, 측정하고, 보호하며, 동적으로 관리함으로써 유용한 지능이 마감시간이 만료되기 전에 일관되게 안전한 물리적 행동으로 전환되도록 해야 한다.

## 06.09. Jitter and Timing Determinism

![](images/image10.png){width="7.268055555555556in" height="7.268055555555556in"}

지터(jitter)와 타이밍 결정성(timing determinism)은 물리 인공지능(Physical AI) 시스템이 의도된 시점에 계산, 통신, 제어를 얼마나 일관되게 수행하는지를 설명한다. 낮은 평균 지연시간(average latency)만으로는 신뢰할 수 있는 실시간 동작(real-time behavior)을 보장할 수 없는데, 개별 실행시간이 평균값을 중심으로 크게 변동할 수 있기 때문이다. 따라서 평균적으로 조금 느리더라도 타이밍이 엄격한 범위 안에서 제한되는 시스템이 평균적으로는 빠르지만 간헐적으로 큰 지연을 발생시키는 시스템보다 물리적 제어에 더 적합할 수 있다.

지터는 반복적으로 발생하는 이벤트의 타이밍이 예상된 스케줄에 비해 변화하는 정도를 의미한다. 주기적 제어 루프(periodic control loop)에서는 연속적인 실행이 일정한 시간 간격으로 시작되거나 종료되지 않을 때 나타난다. 인지(perception)나 계획(planning)에서는 처리 완료시간의 변동으로 나타난다. 통신 지터(communication jitter)는 패킷 도착시간을 변화시키며, 액추에이터 지터(actuator jitter)는 명령 적용 시점을 변화시킨다. 이러한 변동은 센싱-투-액션(sensing-to-action) 파이프라인 전체에 누적되어 최종적인 물리적 반응에 영향을 준다.

타이밍 결정성은 시스템의 타이밍 동작이 정의된 범위 안에서 예측 가능한 상태로 유지되는 것을 의미한다. 모든 계산이 반드시 정확히 동일한 시간을 소비해야 한다는 의미는 아니다. 중요한 요구사항은 실행, 통신, 응답시간이 아키텍처와 제어기가 안전하게 수용할 수 있는 범위 안에 유지되는 것이다. 따라서 결정성은 단순히 평균 계산 속도를 최대화하는 것이 아니라 제한된 동작(bounded behavior)과 예측 가능성(predictability)을 중요하게 다룬다.

평균 지연시간은 타이밍 문제를 숨길 수 있다. 예를 들어 인지 모듈(perception module)이 일반적으로 20밀리초 안에 완료되지만 자원 경합(resource contention)이나 비정상적으로 복잡한 장면으로 인해 간헐적으로 80밀리초가 필요할 수 있다. 평균 성능은 허용 가능한 것처럼 보이더라도 긴 꼬리 지연(long-tail delay)으로 인해 후속 계획 시스템이 오래된 정보(stale information)를 기반으로 동작할 수 있다. 따라서 물리 인공지능은 산술 평균뿐만 아니라 지연시간 분포(latency distribution), 백분위 값(percentile value), 최대 관측 지연시간(maximum observed delay), 마감시간 위반율(deadline-miss rate)을 평가해야 한다.

지터는 운영체제 스케줄링(operating-system scheduling)에서 발생할 수 있다. 범용 운영체제(general-purpose operating system)는 로봇의 마감시간과 반드시 조정되지 않는 많은 프로세스, 인터럽트(interrupt), 드라이버(driver), 백그라운드 서비스(background service)를 실행한다. 제어 또는 인지 스레드(thread)가 다른 작업에 의해 일시적으로 선점(preemption)되어 실행시간이 달라질 수 있다. 실시간 스케줄링(real-time scheduling), 작업 우선순위(task priority), CPU 어피니티(CPU affinity), 인터럽트 관리(interrupt management), 전용 프로세싱 코어(dedicated processing core)를 사용하면 타이밍 중요 기능의 이러한 변동 원인을 줄일 수 있다.

동적 메모리 동작(dynamic memory behavior)은 비결정성(nondeterminism)의 또 다른 원인이다. 메모리 할당(memory allocation), 해제(deallocation), 페이징(paging), 캐시 미스(cache miss), 가비지 컬렉션(garbage collection), 예측하기 어려운 메모리 접근 패턴은 실행시간 변동을 발생시킬 수 있다. 실시간 소프트웨어는 중요 루프(critical loop) 내부에서 동적 할당을 피하고 버퍼와 데이터 구조를 사전에 할당(preallocation)하는 경우가 많다. 예측 가능한 메모리 레이아웃(memory layout)과 제한된 큐(bounded queue)는 일반적인 물리적 제어 과정에서 예상하지 못한 고비용 메모리 연산이 갑자기 발생할 가능성을 줄인다.

현대적인 인공지능 가속기(AI accelerator)는 추가적인 타이밍 변동성을 발생시킨다. GPU 커널(GPU kernel)은 실행 자원, 메모리 대역폭(memory bandwidth), 캐시 용량(cache capacity), 데이터 전송 채널(data-transfer channel)을 놓고 서로 경쟁할 수 있다. 인지, 월드 모델 예측(world-model prediction), 지도 작성(mapping), 계획이 하나의 가속기를 공유하면 서로 간섭할 수 있다. 각각의 모델이 독립적으로 실행될 때 우수한 성능을 보이더라도 동시 실행(concurrent execution)은 가변적인 완료시간을 발생시킬 수 있다. 따라서 자원 분할(resource partitioning)과 신중하게 통제된 스케줄링은 물리 인공지능 공동 설계(co-design)의 중요한 구성요소이다.

입력 의존적 계산(input-dependent computation)도 지터를 발생시킬 수 있다. 희소한 LiDAR 장면은 밀집된 장면과 다른 처리량을 요구할 수 있고, 객체 수가 증가하면 추적 비용(tracking cost)이 증가할 수 있으며, 복잡한 환경에서는 계획 복잡도(planning complexity)가 급격하게 증가할 수 있다. 토큰 기반 추론(token-based reasoning)은 상황에 따라 서로 다른 시퀀스 길이(sequence length)를 요구할 수 있다. 계산량이 입력 복잡도에 크게 의존하는 알고리즘이 시간에 민감한 물리적 의사결정에 사용된다면 명시적인 실행 한계(execution limit)를 설정해야 한다.

통신 시스템(communication system) 역시 고유한 형태의 지터를 발생시킨다. Ethernet 트래픽, 무선 네트워크(wireless network), CAN 중재(CAN arbitration), 스위치 큐(switch queue), 재전송(retransmission), 미들웨어 스케줄링(middleware scheduling), 패킷 버퍼링(packet buffering)은 메시지가 불규칙한 시간 간격으로 도착하게 만들 수 있다. 센서, 컴퓨팅 노드(compute node), 제어기가 로봇 내부에 분산되어 있을 때 특히 중요하다. 우선순위 메커니즘(priority mechanism), 트래픽 셰이핑(traffic shaping), 동기화 네트워크(synchronized network), 예약 대역폭(reserved bandwidth), 제한된 큐, 결정론적 산업용 통신(deterministic industrial communication)을 통해 통신시간 변동을 줄일 수 있다.

센서 타이밍(sensor timing) 역시 상태 추정(state estimation)과 제어를 위해 충분한 결정성을 가져야 한다. 카메라는 서로 약간 다른 시점에 노출될 수 있고, LiDAR 스캔은 일정 시간 동안 측정값을 누적하며, IMU 샘플은 훨씬 높은 주파수로 도착한다. 정확한 타임스탬프(timestamp)와 동기화된 클록(synchronized clock)이 없다면 서로 다른 시점의 측정값을 동시에 발생한 것으로 잘못 처리할 수 있다. 이 경우 타이밍 불확실성(timing uncertainty)이 상태 불확실성(state uncertainty)으로 전환된다. 따라서 클록 동기화(clock synchronization)와 명시적인 측정 시점 처리(measurement-time handling)는 결정론적 센서 융합(deterministic sensor fusion)의 기본 요소이다.

액추에이터 타이밍(actuator timing)은 이 문제의 최종 단계를 나타낸다. 명령이 제시간에 계산되더라도 버스 스케줄링(bus scheduling), 모터 드라이브 주기(motor-drive cycle), 버퍼링, 제어기 동기화(controller synchronization)로 인해 실제 적용이 늦어질 수 있다. 따라서 인공지능 계산 자체가 안정적이더라도 최종 물리적 반응시간은 달라질 수 있다. 종단간 결정성(end-to-end determinism)을 확보하려면 소프트웨어가 명령 생성을 완료하는 시점에서 측정을 끝내지 말고 액추에이터 인터페이스와 기계적 응답(mechanical response)까지 타이밍을 분석해야 한다.

지터는 가변적인 지연이 실질적인 샘플링 및 피드백 간격을 변화시키기 때문에 제어 이론적(control-theoretic) 결과를 직접적으로 발생시킨다. 고정 주기(fixed period)를 기준으로 설계된 제어기는 측정과 명령이 예측 가능한 시점에 발생한다고 가정한다. 타이밍 변동은 위상 불확실성(phase uncertainty)을 발생시키며 안정성 여유(stability margin), 추종 품질(tracking quality), 외란 억제(disturbance rejection) 성능을 저하시킬 수 있다. 작은 타이밍 변동은 허용될 수 있지만 시스템 동역학(system dynamics)이 제어 주기에 비해 빠른 경우 크거나 반복적인 지터는 심각한 문제가 될 수 있다.

물리 인공지능의 서로 다른 부분에는 서로 다른 수준의 타이밍 결정성이 요구된다. 모터 전류 제어(motor current regulation), 안정화(stabilization), 안전 모니터링(safety monitoring), 비상 기능(emergency function)은 강하게 제한된 타이밍을 요구할 수 있다. 인지와 지역 계획(local planning)은 정보가 충분히 최신 상태로 유지되는 한 일정 수준의 변동을 허용할 수 있다. 지도 작성, 의미론적 해석(semantic interpretation), 장기 추론(long-horizon reasoning)은 훨씬 더 큰 변동성을 허용할 수 있다. 따라서 전체 자율성 스택(autonomy stack)에 동일한 타이밍 보장을 적용하는 대신 물리적 결과의 중요성에 따라 타이밍 보장을 할당해야 한다.

격리(isolation)는 결정론적 기능을 보호하는 가장 효과적인 방법 중 하나이다. 저수준 제어(low-level control)와 안전 작업(safety task)은 전용 MCU, 실시간 프로세서(real-time processor), FPGA 또는 예약된 CPU 코어에서 실행하고, 변동성이 높은 인공지능 워크로드는 GPU 또는 다른 가속기에서 별도로 실행할 수 있다. 이러한 영역 사이의 통신은 제한된 큐, 타임스탬프, 유효성 한계(validity limit), 정의된 갱신 주기(update rate)를 가진 명시적인 인터페이스를 사용하여 가변적인 인공지능 타이밍이 물리적 제어를 직접 불안정하게 만들지 않도록 해야 한다.

다중 주기 아키텍처(multi-rate architecture)는 인공지능 지터의 영향을 더욱 감소시킨다. 고수준 정책(high-level policy)이 수십 헤르츠로 새로운 궤적(trajectory)을 제공하는 동안 결정론적 제어기는 수백 또는 수천 헤르츠로 해당 궤적을 추종할 수 있다. 하나의 인공지능 갱신이 약간 늦더라도 내부 제어기(inner controller)는 가장 최근의 유효한 기준값(reference)을 사용하여 계속 실행된다. 이러한 시간적 분리(temporal decoupling)는 신경망 추론시간(neural inference time)의 모든 변동이 액추에이터 명령 타이밍의 동일한 변동으로 직접 전달되는 것을 방지한다.

그러나 오래된 기준값을 무한정 계속 사용하는 것은 안전하지 않다. 모든 명령, 궤적, 인지 결과 또는 월드 상태(world state)는 명시적인 정보 최신성 한계(freshness limit)를 가져야 한다. 타임스탬프를 사용하면 후속 구성요소가 정보 연령(information age)을 계산할 수 있으며, 타임아웃 정책(timeout policy)은 출력이 언제 더 이상 유효하지 않은지를 결정한다. 유효성 임계값(validity threshold)을 초과하면 시스템은 오래된 정보를 거부하고 미리 정의된 성능 저하 모드(degraded mode), 보수적 행동(conservative behavior) 또는 안전 대체 동작(safe fallback)을 활성화해야 한다.

버퍼링(buffering)은 단기적인 타이밍 변동을 줄이는 동시에 지연시간을 증가시킬 수 있기 때문에 신중하게 처리해야 한다. 큐는 불규칙한 데이터 도착률을 완화할 수 있지만 깊은 큐(deep queue)는 오래된 정보가 시스템에 계속 남도록 만든다. 실시간 물리 인공지능에서는 무제한 큐(unlimited queue)보다 제한된 버퍼(bounded buffer) 또는 최신값 버퍼(latest-value buffer)가 더 적합한 경우가 많다. 목표는 모든 샘플을 보존하는 것이 아니라 일시적인 계산량 증가가 지속적인 지연으로 전환되는 것을 방지하면서 충분히 최신의 상태 정보를 유지하는 것이다.

타이밍 결정성은 통계적(statistical) 방법과 구조적(structural) 방법을 함께 사용하여 평가해야 한다. 유용한 측정값에는 평균 지연시간(mean latency), 표준편차(standard deviation), P95, P99, 최대 관측 지연시간, 최대 지터(maximum jitter), 정보 연령, 마감시간 위반 확률(deadline-miss probability)이 포함된다. 하드 실시간(hard real-time) 기능에서는 통계적 증거만으로 충분하지 않을 수 있으며 분석적 또는 구조적으로 제한된 실행 동작이 필요할 수 있다. 상대적으로 소프트한 인공지능 기능에서는 백분위 측정값이 현실적인 워크로드에서 긴 꼬리 성능(long-tail performance)을 파악하는 실용적인 지표가 된다.

스트레스 테스트(stress testing)는 유휴 상태의 실험실 시스템에서 관측되는 타이밍 동작이 실제 배포 환경과 크게 다를 수 있기 때문에 필수적이다. 인지, 월드 모델링, 계획, 로깅(logging), 네트워킹(networking), 저장장치(storage), 시각화(visualization), 제어가 동시에 실행되는 상태에서 CPU, GPU, 메모리, 통신, 열 부하(thermal load)를 현실적인 최대 수준에 가깝게 증가시켜야 한다. 목적은 로봇 운용 과정에서 물리적 실패로 이어지기 전에 자원 경합 경로(contention path)와 드물게 발생하는 지연을 찾아내는 것이다.

적응형 워크로드 관리(adaptive workload management)는 자원 압력이 증가할 때 타이밍 결정성을 보호할 수 있다. 시스템은 인지 해상도(perception resolution)를 낮추고, 중요하지 않은 프레임을 건너뛰고, 월드 모델 롤아웃(world-model rollout)을 단축하고, 추론 깊이(reasoning depth)를 제한하고, 계획 샘플 수를 줄이고, 지도 작성을 연기하거나, 백그라운드 작업을 비활성화할 수 있다. 타이밍 중요 제어(timing-critical control)는 우선순위를 유지한다. 충분한 타이밍 여유를 계속 확보할 수 없다면 로봇의 속도를 낮추거나 더 안전한 운용 모드(safe operating mode)로 전환하여 물리적 응답시간 요구사항을 완화할 수 있다.

궁극적으로 지터(jitter)와 타이밍 결정성(timing determinism)은 물리 인공지능이 단순히 빠르게 계산하는 것을 넘어 시간적으로 예측 가능한 방식으로 동작할 수 있는지를 결정한다. 신뢰할 수 있는 물리적 구현(reliable embodiment)을 위해서는 제한된 실행시간(bounded execution), 동기화된 센싱(synchronized sensing), 통제된 통신(controlled communication), 예측 가능한 명령 전달(predictable command delivery), 가변적인 인공지능 워크로드로부터 중요 제어 루프를 격리하는 구조가 필요하다. 따라서 공학적 목표는 단순히 최소 평균 지연시간을 달성하는 것이 아니라 현실적인 운용 조건에서도 안전한 물리적 행동을 수행할 수 있도록 센싱-투-액션 시스템의 타이밍을 충분히 안정적이고 제한된 범위 안에 유지하는 것이다.

## 06.10. Communication and Network Latency

![](images/image11.png){width="7.268055555555556in" height="7.268055555555556in"}

통신 및 네트워크 지연시간(communication and network latency)은 센서, 컴퓨팅 노드(compute node), 제어기(controller), 액추에이터(actuator), 로봇, 엣지 서버(edge server), 그리고 물리 인공지능(Physical AI) 시스템의 다른 구성요소 사이에서 정보가 이동하는 데 필요한 시간이다. 단일 프로세서 내부의 지연시간과 달리 네트워크 지연은 전송(transmission), 스위칭(switching), 라우팅(routing), 직렬화(serialization), 버퍼링(buffering), 프로토콜 처리(protocol processing), 자원 경합(contention)의 영향을 받는다. 분산 지능(distributed intelligence)은 적시에 이루어지는 데이터 교환에 의존하기 때문에 통신 지연시간은 단순한 인프라 문제가 아니라 물리적인 센싱-투-액션(sensing-to-action) 루프의 일부가 된다.

물리 인공지능 플랫폼은 일반적으로 여러 통신 영역(communication domain)을 동시에 포함한다. 카메라와 LiDAR는 Ethernet을 통해 고대역폭 센서 스트림(high-bandwidth sensor stream)을 전송할 수 있고, 임베디드 제어기(embedded controller)는 CAN이나 EtherCAT을 통해 결정론적 명령(deterministic command)을 교환하며, 무선 링크(wireless link)는 로봇을 플릿 서버(fleet server) 또는 다른 로봇과 연결할 수 있다. 각각의 영역은 대역폭, 신뢰성(reliability), 범위, 타이밍, 결정성(determinism) 특성이 서로 다르다. 따라서 네트워크 아키텍처는 각 기능의 타이밍 중요도(timing criticality)에 적합한 통신 기술을 선택해야 한다.

네트워크 지연시간은 하나의 단순한 전송 지연(transmission delay)이 아니라 여러 구성요소로 이루어진다. 데이터는 먼저 통신 링크에 직렬화되고, 물리적 매체(physical medium)를 통해 전파된 후 인터페이스에서 처리되고, 스위치 또는 제어기의 큐(queue)에 저장되며, 소프트웨어 프로토콜을 거쳐 목적지 애플리케이션(destination application)으로 전달된다. 추가적인 데이터 복사, 미들웨어 처리(middleware processing), 암호화(encryption), 재전송(retransmission), 동기화(synchronization)도 지연시간을 증가시킬 수 있다. 따라서 종단간 분석(end-to-end analysis)은 데이터 생성부터 실제 소비까지의 전체 경로를 포함해야 한다.

직렬화 지연시간(serialization latency)은 메시지 크기와 링크 전송률(link rate)에 따라 달라진다. 대용량 카메라 영상, 포인트 클라우드(point cloud), 점유 지도(occupancy map), 모델 특징(model feature)은 작은 제어 명령보다 더 많은 전송시간을 요구한다. 네트워크 대역폭을 높이면 직렬화 시간을 줄일 수 있지만 큐잉(queueing), 프로토콜, 처리 지연까지 자동으로 제거되는 것은 아니다. 따라서 물리 인공지능 통신은 단순한 링크 용량뿐만 아니라 메시지 표현(message representation), 압축(compression), 패킷화(packetization), 갱신 주파수(update frequency), 각 인터페이스를 통과해야 하는 정보량까지 최적화해야 한다.

대역폭(bandwidth)과 지연시간(latency)은 서로 관련되어 있지만 근본적으로 다른 특성이다. 네트워크가 초당 많은 양의 데이터를 전송할 수 있더라도 개별 메시지에는 상당한 지연이 발생할 수 있다. 반대로 상대적으로 낮은 대역폭의 제어 버스(control bus)가 작은 명령에 대해 매우 예측 가능한 전달을 제공할 수도 있다. 따라서 고대역폭 인지 파이프라인(high-bandwidth perception pipeline)과 저지연 제어 네트워크(low-latency control network)는 서로 다른 최적화 목표를 요구한다. 시스템 설계에서는 대역폭을 통신 성능의 보편적인 척도로 취급하지 말고 처리량(throughput), 지연시간, 지터(jitter), 패킷 손실(packet loss), 결정성을 각각 명시해야 한다.

큐잉 지연시간(queueing latency)은 네트워크 지연에서 가장 크고 예측하기 어려운 요소 중 하나가 될 수 있다. 여러 센서 또는 컴퓨팅 프로세스가 동일한 링크의 전송 능력보다 빠르게 데이터를 보내면 패킷이 버퍼에 누적된다. 개별 알고리즘은 정상 속도로 계속 실행될 수 있지만 정보 연령(information age)은 네트워크 내부에서 보이지 않게 증가한다. 통신 혼잡(communication congestion)으로 인해 현재 관측이 오래된 정보(stale information)로 변하는 것을 방지하려면 제한된 큐(bounded queue), 트래픽 우선순위화(traffic prioritization), 최신 데이터 정책(latest-data policy), 전송률 제어(rate control), 충분한 네트워크 용량이 필요하다.

통신 지연시간은 일반적으로 시간에 따라 변화하기 때문에 지터는 특히 중요하다. 네트워크 트래픽, 스위치 경합(switch contention), 무선 간섭(wireless interference), 운영체제 스케줄링(operating-system scheduling), 재전송, 경쟁 애플리케이션(competing application)은 패킷 도착 간격을 변화시킬 수 있다. 따라서 주기적인 갱신을 기대하는 제어기는 여러 패킷을 짧은 시간 안에 연속으로 수신한 후 긴 공백을 경험할 수도 있다. 물리 인공지능 시스템에서는 특히 네트워크 메시지가 움직임이나 안전에 영향을 주는 경우 평균 지연뿐만 아니라 백분위 지연시간(percentile latency)과 최악 조건 통신 지연시간(worst-case communication latency)을 평가해야 한다.

패킷 손실은 또 다른 시간적 문제를 발생시킨다. 신뢰성 프로토콜(reliable protocol)은 누락된 데이터를 재전송하여 데이터 완전성을 높일 수 있지만 지연시간도 증가시킨다. 그러나 일부 실시간 애플리케이션에서는 오래된 재전송 센서 프레임보다 가장 최신의 프레임이 더 가치가 있다. 따라서 통신 정책은 데이터의 의미(data semantics)를 반영해야 한다. 구성 명령(configuration command)이나 중요한 상태 전환(critical state transition)은 신뢰성 있는 전달이 필요할 수 있지만, 고주파 인지 스트림(high-rate perception stream)은 오래된 정보를 기다리는 대신 간헐적인 패킷 손실을 허용하고 정보 최신성(freshness)을 우선할 수 있다.

유선 통신(wired communication)은 일반적으로 무선 통신보다 높은 타이밍 예측 가능성을 제공하지만, 그 특성은 여전히 프로토콜과 아키텍처에 따라 달라진다. Ethernet은 고대역폭 센서 전송을 지원할 수 있으며, 결정론적 확장(deterministic extension)과 산업용 Ethernet 기술은 보다 엄격한 스케줄링을 제공할 수 있다. CAN과 유사한 필드버스(field bus)는 작은 제어 및 상태 메시지에 효과적이다. 적절한 기술은 메시지 크기, 갱신 주파수, 토폴로지(topology), 전자기 환경(electromagnetic environment), 요구되는 이중화(redundancy), 허용 가능한 타이밍 변동에 따라 결정된다.

무선 통신은 채널 상태가 거리, 장애물, 간섭, 이동성(mobility), 다중경로 전파(multipath propagation), 네트워크 혼잡에 따라 변화하기 때문에 추가적인 불확실성을 발생시킨다. Wi-Fi, 셀룰러 네트워크(cellular network), 프라이빗 5G(private 5G) 등의 무선 기술은 유용한 연결성을 제공하지만 항상 엄격하게 제한된 지연시간을 보장할 수 있는 것은 아니다. 따라서 안전 중요 액추에이터 안정화(safety-critical actuator stabilization)는 예측하기 어려운 무선 경로에만 의존해서는 안 된다. 외부 연결이 지연되거나 사용할 수 없는 경우에도 로컬 자율성(local autonomy)이 안전한 동작을 유지할 수 있어야 한다.

엣지 컴퓨팅(edge computing)은 인지, 월드 모델링(world modeling), 계획(planning), 제어를 로봇 가까이에 배치하여 네트워크 의존성을 줄일 수 있다. 원시 센서 데이터(raw sensor data)는 로컬에서 처리하고 압축된 상태, 이벤트(event), 특징(feature), 지도, 작업 정보만 외부로 전송할 수 있다. 이를 통해 대역폭 요구량을 줄이고 시간 중요 의사결정(time-critical decision)을 위해 원격 서버까지 왕복하는 과정을 피할 수 있다. 따라서 엣지-클라우드 분할(edge-cloud partitioning)은 각 기능이 통신 지연과 일시적인 네트워크 단절을 허용할 수 있는지에 따라 결정되어야 한다.

원격 또는 클라우드 계산(remote or cloud computation)은 즉각적인 물리적 반응이 필요하지 않은 워크로드에 여전히 유용하다. 플릿 최적화(fleet optimization), 대규모 지도 처리, 이력 분석(historical analysis), 모델 학습(model training), 장기 추론(long-horizon reasoning), 데이터 집계(data aggregation), 소프트웨어 업데이트는 원격 자원을 효과적으로 사용할 수 있다. 그러나 기능을 원격에 배치하면 업링크(uplink), 라우팅, 서버 큐잉(server queueing), 계산, 다운링크(downlink) 지연이 추가된다. 따라서 시스템은 전체 원격 왕복시간(remote round-trip time)을 요청된 결과가 물리적으로 유효한 시간과 비교해야 한다.

온프레미스 컴퓨팅(on-premise computing)은 로봇 로컬 엣지 컴퓨팅(robot-local edge computing)과 원거리 클라우드 인프라 사이의 중간 위치를 차지할 수 있다. 시설 내부에 강력한 공유 가속기(shared accelerator)를 배치하면 퍼블릭 클라우드(public cloud)보다 낮고 제어하기 쉬운 네트워크 지연시간을 제공할 수 있다. 여러 로봇은 로컬 안전 및 제어 기능을 유지하면서 계산 집약적인 작업(computationally intensive task)을 오프로딩(offloading)할 수 있다. 그러나 네트워크 장애나 서버 과부하는 여전히 발생할 수 있으므로 중요 기능은 중앙집중형 지능(centralized intelligence)에 대한 지속적인 접근을 가정하지 않고 명확한 로컬 대체 동작(local fallback behavior)을 유지해야 한다.

다중 로봇 물리 인공지능(multi-robot Physical AI)은 일반적인 센서 네트워킹을 넘어서는 통신 의존성을 발생시킨다. 로봇은 자세(pose), 지역 지도(local map), 궤적(trajectory), 검출 객체(detected object), 작업 상태(task state), 협조 메시지(coordination message)를 서로 교환할 수 있다. 지연시간은 한 로봇이 다른 로봇의 현재 상태를 얼마나 정확하게 이해하는지에 영향을 준다. 지연된 궤적이나 자세 정보는 이미 상당한 거리를 이동한 로봇의 과거 상태를 나타낼 수 있다. 따라서 협력 자율성(cooperative autonomy)은 공유 정보를 사용할 때 메시지 연령(message age), 불확실성, 예측, 통신 품질을 명시적으로 고려해야 한다.

분산 위치 추정 및 지도 작성(distributed localization and mapping)은 특히 네트워크 설계에 민감하다. 로봇 사이에서 원시 카메라 또는 LiDAR 데이터를 전송하면 막대한 대역폭이 필요할 수 있지만, 압축된 특징, 키프레임(keyframe), 서브맵(submap), 자세 제약조건(pose constraint)을 교환하면 통신 요구량을 줄일 수 있다. 따라서 협업을 위해 어떤 표현을 선택하는가 자체가 지연시간 및 대역폭 설계 결정이 된다. 통신 인식 알고리즘(communication-aware algorithm)은 무제한 네트워크 용량을 가정하는 대신 유용한 공유 상태(shared state)를 유지하는 데 필요한 최소한의 정보를 전송해야 한다.

시간 동기화(time synchronization)는 측정값과 상태가 분산 노드(distributed node) 사이에서 이동하는 모든 경우에 기본적으로 중요하다. 패킷 도착 타임스탬프(arrival timestamp)만으로는 해당 데이터가 나타내는 물리적 사건이 실제로 언제 발생했는지 알 수 없다. 센서와 컴퓨터는 동기화된 클록(synchronized clock)을 공유하여 수신 모듈이 정보 연령을 판단하고 관측을 올바르게 정렬할 수 있도록 해야 한다. PTP, 하드웨어 타임스탬핑(hardware timestamping) 또는 기타 동기화 메커니즘을 통해 이를 지원할 수 있다. 정확한 시간 정렬(time alignment)이 없다면 통신 지연을 실제 물리적 움직임이나 상태 추정 오류(state-estimation error)로 잘못 해석할 수 있다.

네트워크 토폴로지(network topology) 역시 지연시간과 회복탄력성(resilience)에 영향을 준다. 중앙집중형 아키텍처(centralized architecture)는 협조를 단순화할 수 있지만 병목현상(bottleneck)과 단일 장애점(single point of failure)을 만들 수 있다. 분산 아키텍처(distributed architecture)는 하나의 노드에 대한 의존성을 줄일 수 있지만 더욱 복잡한 동기화와 상태 일관성(state consistency)을 요구한다. 이중화 링크(redundant link)는 장애 허용성(fault tolerance)을 향상시킬 수 있지만 라우팅과 관리 복잡도를 증가시킨다. 따라서 물리 인공지능의 토폴로지는 데이터 흐름(data flow), 타이밍 중요도, 장애 모드(failure mode), 물리적 배선 제약조건, 요구되는 운용 가용성(operational availability)을 기준으로 설계해야 한다.

서비스 품질 메커니즘(quality-of-service mechanism)은 네트워크를 공유하는 경우 중요 트래픽을 보호할 수 있다. 안전 메시지(safety message), 제어 기준값(control reference), 동기화 패킷, 인지 스트림, 로깅 트래픽(logging traffic), 소프트웨어 업데이트가 반드시 동일한 우선순위를 가져야 하는 것은 아니다. 트래픽 클래스(traffic class), 메시지 우선순위, 대역폭 예약(bandwidth reservation), 스케줄링, 전송률 제한(rate limiting)을 사용하면 백그라운드 전송(background transfer)이 시간에 민감한 통신을 지연시키는 것을 방지할 수 있다. 물리적 행동이 메시지 전달에 의존하는 경우 네트워크 스케줄러(network scheduler)는 사실상 실시간 아키텍처(real-time architecture)의 일부가 된다.

통신 인터페이스에는 명시적인 시간적 의미 정보(temporal semantics)가 포함되어야 한다. 메시지에는 획득 타임스탬프(acquisition timestamp), 시퀀스 번호(sequence number), 유효 기간(validity interval), 소스 식별자(source identifier), 신뢰도 정보(confidence information)를 포함할 수 있다. 이를 통해 수신 시스템은 새롭게 생성된 상태와 지연되거나 중복된 패킷을 구분할 수 있다. 정보가 허용된 최대 연령(maximum permitted age)을 초과하면 무조건 적용하는 대신 거부할 수 있다. 따라서 시간 메타데이터(temporal metadata)는 분산 물리 인공지능 구성요소가 전달된 정보가 여전히 현재의 물리 세계를 나타내는지를 판단할 수 있도록 한다.

통신 품질이 저하될 때는 점진적 성능 저하(graceful degradation)가 필요하다. 로봇은 원격 인지(remote perception)에 대한 의존성을 줄이고, 협력 위치 추정(cooperative localization)에서 로컬 위치 추정(local localization)으로 전환하고, 전송 데이터율(data rate)을 낮추고, 협조 복잡도(coordination complexity)를 줄이거나, 로컬에서 검증된 궤적을 계속 실행할 수 있다. 필수적인 협조 정보를 사용할 수 없게 되면 속도를 낮추고, 안전 여유를 확대하거나 정지할 수 있다. 따라서 네트워크 성능 저하는 자율성의 통제되지 않은 상실이 아니라 정의된 기능 저하로 이어져야 한다.

통신 성능은 거의 사용되지 않는 유휴 네트워크에서가 아니라 현실적인 부하(realistic load) 조건에서 검증해야 한다. 시험에는 전체 센서 트래픽, 다중 로봇 메시지, 로깅, 원격 계산, 동기화, 제어 트래픽, 백그라운드 전송을 동시에 포함해야 한다. 엔지니어는 단방향 및 왕복 지연시간(one-way and round-trip latency), 지터, 패킷 손실, 큐 깊이(queue depth), 처리량, 정보 연령, 마감시간 위반(deadline miss)을 측정해야 한다. 무선 시스템은 추가적으로 이동성, 간섭, 약한 통신 범위(weak coverage), 핸드오버(handover), 일시적인 연결 단절 조건에서도 시험해야 한다.

궁극적으로 통신 및 네트워크 지연시간(communication and network latency)은 물리 인공지능의 분산된 구성요소가 물리 세계에 대한 일관된 표현을 얼마나 빠르게 공유하고 행동을 조정할 수 있는지를 결정한다. 신뢰할 수 있는 시스템은 충분한 대역폭과 함께 제한된 지연시간(bounded latency), 낮은 지터(low jitter), 동기화된 클록, 우선순위화된 트래픽(prioritized traffic), 정보 최신성을 고려한 메시징(freshness-aware messaging), 로컬 대체 기능(local fallback capability)을 결합한다. 따라서 네트워크 설계는 물리 인공지능 아키텍처와 분리할 수 없다. 지능은 여러 프로세서와 로봇에 분산될 수 있지만 안전한 물리적 행동은 유효성을 잃을 정도로 늦게 도착한 정보에 의존해서는 안 된다.

## 06.11. Graceful Degradation under Compute Overload

![](images/image12.png){width="7.268055555555556in" height="7.268055555555556in"}

![](images/image13.png){width="7.268055555555556in" height="7.268055555555556in"}

컴퓨팅 과부하 상황에서의 점진적 성능 저하(graceful degradation under compute overload)는 사용 가능한 컴퓨팅 자원(computational resources)이 전체 자율성 워크로드(autonomy workload)를 더 이상 지원할 수 없을 때에도 물리 인공지능(Physical AI) 시스템이 안전하고 유용한 동작을 유지하는 능력을 의미한다. 과부하로 인해 통제되지 않는 지연시간, 오래된 정보(stale information), 마감시간 위반(deadline miss), 완전한 기능 실패가 발생하도록 두는 대신 시스템은 의도적으로 계산 품질이나 기능 수준을 낮춘다. 핵심 목표는 물리적 안전과 제어에 필요한 타이밍 보장을 상실하기 전에 중요하지 않은 지능 기능을 먼저 희생하는 것이다.

컴퓨팅 과부하(compute overload)는 요구되는 처리량이 CPU, GPU, NPU, 메모리 시스템, 통신 인터페이스 또는 기타 컴퓨팅 자원이 지속적으로 제공할 수 있는 처리 능력을 초과할 때 발생한다. 예상보다 복잡한 장면, 여러 인공지능 모델의 동시 실행, 증가한 센서 트래픽, 다중 에이전트 상호작용(multi-agent interaction), 백그라운드 작업, 열 스로틀링(thermal throttling), 일시적인 자원 경합(resource contention) 등이 원인이 될 수 있다. 물리 인공지능은 변화하는 환경에서 동작하므로 최대 계산 수요가 때때로 정상 설계 조건을 초과할 수 있다는 점을 전제로 아키텍처를 설계해야 한다.

과부하는 단순히 높은 자원 사용률(high utilization)과는 다르며, 가장 중요한 결과는 증가하는 지연시간이다. 프로세서가 거의 최대 용량으로 동작하더라도 모든 중요 작업이 마감시간을 충족한다면 허용될 수 있다. 그러나 입력 작업이 처리 속도보다 빠르게 누적되기 시작하면 큐(queue)가 증가하고 정보는 점점 오래된다. 따라서 로봇은 겉보기에는 올바른 인지 및 계획 결과를 계속 생성하면서도 실제로는 안전한 행동에 사용하기에는 너무 오래된 물리적 상태를 나타내는 결과를 사용할 수 있다.

점진적 성능 저하를 위한 첫 번째 요구사항은 컴퓨팅 상태(computational health)를 지속적으로 파악하는 것이다. 시스템은 프로세서 사용률, 가속기 점유율(accelerator occupancy), 메모리 대역폭(memory bandwidth), 큐 깊이(queue depth), 추론 지연시간(inference latency), 프레임 연령(frame age), 제어 마감시간(control deadline), 네트워크 혼잡(network congestion), 열 상태(thermal state), 전력 한계(power limit)를 모니터링할 수 있다. 하나의 지표만으로는 충분하지 않다. 높은 GPU 사용률은 문제가 아닐 수도 있지만 정보 연령의 증가나 반복적인 마감시간 위반은 센싱-투-액션(sensing-to-action) 파이프라인이 물리 환경의 변화 속도를 따라가지 못하고 있음을 의미한다.

타이밍 중요도(timing-criticality)는 과부하 상황에서 어떤 워크로드를 보호할 것인지를 결정해야 한다. 저수준 안정화(low-level stabilization), 액추에이터 제어(actuator control), 비상 모니터링(emergency monitoring), 워치독(watchdog), 필수 통신은 가장 강력한 자원 보장을 받아야 한다. 지역 인지(local perception)와 충돌 관련 계획(collision-related planning)은 그다음 수준으로 보호할 수 있으며, 의미론적 추론(semantic reasoning), 고해상도 지도 작성(high-resolution mapping), 장기 예측(long-horizon prediction), 로깅(logging), 시각화(visualization), 백그라운드 학습(background learning)은 먼저 축소할 수 있다. 따라서 성능 저하 정책은 단순한 계산 비용이 아니라 물리적 결과의 중요성을 반영해야 한다.

자원 격리(resource isolation)는 가변적인 인공지능 워크로드가 중요 제어에 필요한 자원을 소비하는 것을 방지한다. 안전 기능은 전용 MCU, 실시간 CPU(real-time CPU), FPGA, 예약된 프로세서 코어 또는 독립적인 제어기에서 실행할 수 있으며, 인지와 추론은 공유 가속기(shared accelerator)를 사용할 수 있다. 메모리, 통신 대역폭, 스케줄링 우선순위도 분할할 수 있다. 이러한 분리를 통해 과부하된 월드 모델(world model)이나 인지 네트워크가 로봇의 액추에이터 안정성 유지 또는 안전 정지(safe stop) 실행을 직접 방해하지 못하도록 한다.

인지(perception)는 여러 가지 실용적인 성능 저하 메커니즘을 제공한다. 시스템은 카메라 해상도를 낮추고, 프레임률(frame rate)을 감소시키고, 처리하는 카메라 수를 줄이고, 전처리(preprocessing)를 단순화하고, 더 작은 신경망을 선택하고, 검출 클래스(detection class)를 축소하거나 중요하지 않은 분할 작업(segmentation task)을 건너뛸 수 있다. 오래된 프레임이 큐에 누적되도록 두는 대신 최신 프레임 처리(latest-frame processing)를 사용하여 오래된 관측을 폐기하고 현재 정보를 우선할 수 있다. 이는 공간적 또는 의미론적 세부 정보를 시간적 최신성(temporal freshness)과 교환하는 것으로, 물리적 상호작용에서는 종종 더 안전한 선택이다.

센서 처리(sensor processing)도 상황 의존적(context-dependent)으로 변경할 수 있다. 환경이 단순하고 위험도가 낮을 때는 축소된 센싱 구성(reduced sensing configuration)만으로 충분할 수 있다. 장애물, 사람, 불확실한 지형 또는 빠른 움직임으로 위험이 증가하면 가장 관련성이 높은 센서와 인지 기능에 컴퓨팅 자원을 재할당할 수 있다. 따라서 적응형 센서 스케줄링(adaptive sensor scheduling)을 사용하면 모든 센싱 모달리티(sensing modality)를 항상 최대 계산 강도로 동작시키지 않고도 중요한 상황 인식(situational awareness)을 유지할 수 있다.

월드 모델(world model)은 예측 복잡도(prediction complexity)를 줄이는 방식으로 성능을 낮출 수 있다. 시스템은 예측 범위(prediction horizon)를 단축하고, 롤아웃 분기(rollout branch)의 수를 줄이고, 공간 해상도(spatial resolution)를 낮추고, 먼 영역의 갱신 빈도를 감소시키거나 계산 비용이 높은 의미론적 예측을 일시적으로 비활성화할 수 있다. 근거리 동역학(near-field dynamics)과 임박한 위험(imminent hazard)은 먼 미래 또는 추측적인 미래보다 높은 우선순위를 유지할 수 있다. 따라서 사용 가능한 컴퓨팅 자원이 감소할수록 월드 모델은 완전히 사라지는 대신 점진적으로 더 국소적이고 보수적인 형태로 변화한다.

추론 워크로드(reasoning workload) 역시 유사한 방식으로 조정할 수 있다. 대규모 멀티모달 모델(large multimodal model)은 추론 깊이(reasoning depth), 컨텍스트 크기(context size), 토큰 생성(token generation), 메모리 검색(memory retrieval), 고려하는 후보 가설(candidate hypothesis)의 수를 줄일 수 있다. 즉각적인 반응 행동(reactive behavior)이 필요한 경우 복잡한 숙고(deliberation)를 일시 중지할 수도 있다. 이는 충분한 시간이 있어 결과를 실제 행동에 사용할 수 있을 때에만 깊은 추론이 가치가 있다는 물리 인공지능의 중요한 원칙을 반영한다. 과부하 상황에서는 늦게 도착하는 정교한 추론보다 적시에 제공되는 근사적 지능(approximate intelligence)이 더 유용할 수 있다.

계획 알고리즘(planning algorithm)은 점진적 성능 저하를 적용할 수 있는 또 다른 중요한 영역이다. 탐색 깊이(search depth), 궤적 샘플(trajectory sample), 최적화 반복 횟수(optimization iteration), 계획 범위(planning horizon), 충돌 검사 해상도(collision-check resolution)를 사용 가능한 시간에 따라 줄일 수 있다. 애니타임 계획기(anytime planner)는 안전하고 실행 가능한 해결책을 먼저 생성하고 자원이 남아 있는 동안 이를 개선할 수 있기 때문에 특히 유용하다. 계산 자원이 제한되면 로봇은 전역적으로 더 우수한 계획을 무한정 기다리는 대신 현재까지 검증된 최선의 해결책을 실행한다.

이전에 유효했던 궤적(previous valid trajectory)은 제한된 시간 동안 재사용할 수 있지만 명시적인 유효성 검사(validity check)가 필요하다. 시스템은 환경 변화, 로봇 움직임 또는 경과 시간으로 인해 해당 궤적이 안전하지 않게 되었는지 판단해야 한다. 환경이 천천히 변화하는 경우 궤적 재사용은 일시적인 과부하 상황에서 계산량을 줄일 수 있다. 그러나 계산 절감이 오래된 가정에 대한 의존으로 이어지지 않도록 지역 충돌 모니터링(local collision monitoring) 및 정보 최신성 한계(freshness limit)와 함께 사용해야 한다.

모델 적응(model adaptation)은 단계적으로 구분된 계산 품질 수준을 제공할 수 있다. 시스템은 대형, 중형, 경량 인지 또는 정책 모델을 유지하고 자원 가용성(resource availability)에 따라 이들을 전환할 수 있다. 동적 신경망(dynamic neural network)은 조기 종료(early exit), 토큰 가지치기(token pruning), 희소 계산(sparse computation), 저정밀도 연산(reduced precision), 조건부 실행(conditional execution)을 사용할 수도 있다. 이러한 메커니즘은 로봇이 최고 성능 인공지능과 완전한 기능 상실 중 하나만 선택하도록 하는 대신 상황에 따라 계산 수요를 확장하거나 축소할 수 있게 한다.

주기 적응(rate adaptation)은 또 다른 제어 수단을 제공한다. 모든 인공지능 모듈이 모든 센서 프레임마다 실행될 필요는 없다. 지도 작성(mapping)은 장애물 검출보다 낮은 주파수로 갱신할 수 있고, 의미론적 추론은 지역 계획(local planning)보다 느리게 실행할 수 있으며, 장기 예측은 일시적으로 건너뛸 수 있다. 다중 주기 스케줄링(multi-rate scheduling)을 사용하면 중요 기능은 갱신 주기를 유지하는 동안 덜 긴급한 기능이 컴퓨팅 자원을 양보할 수 있다. 이를 통해 전체 계산 능력이 정상 운용에 부족해지더라도 시간적 구조(temporal structure)를 유지할 수 있다.

물리적 행동 자체도 컴퓨팅 능력에 맞추어 조정할 수 있다. 로봇이 현재 속도에서 환경을 충분히 빠르게 처리할 수 없다면 속도를 줄임으로써 센싱, 예측, 계획, 반응에 사용할 수 있는 시간을 늘릴 수 있다. 시스템은 추종 거리(following distance)를 증가시키고, 좁은 통로를 피하고, 조작 속도(manipulation speed)를 낮추거나, 더 단순한 경로를 선택할 수도 있다. 따라서 컴퓨팅 관리(compute management)와 모션 계획(motion planning)은 서로 연계되어야 하며, 계산 능력을 증가시킬 수 없을 때 로봇이 물리적 문제의 난이도를 낮출 수 있어야 한다.

성능 저하는 통제되지 않는 성능 붕괴가 아니라 정의된 운용 수준(operating level)을 통해 이루어져야 한다. 정상 모드(nominal mode)에서는 전체 센싱, 월드 모델링, 추론, 계획을 사용할 수 있다. 중간 수준의 과부하 모드(moderate overload mode)에서는 중요하지 않은 해상도와 갱신 주기를 줄일 수 있다. 심각한 과부하 모드(severe overload mode)에서는 필수적인 지역 인지, 제어, 안전 기능만 유지할 수 있다. 최소한의 안전 계산(minimum safe computation)조차 보장할 수 없다면 최종 상태는 알려지지 않은 타이밍 특성으로 계속 운용하는 것이 아니라 통제된 안전 정지가 되어야 한다.

히스테리시스(hysteresis)는 성능 저하 수준 사이를 전환할 때 유용하다. 이를 사용하지 않으면 자원 임계값(resource threshold) 근처에서 동작하는 시스템이 정상 모드와 축소 모드 사이를 반복적으로 전환하여 불안정한 계산 동작을 만들 수 있다. 성능 저하 모드로의 진입은 상대적으로 높은 사용률 또는 지연시간 임계값에서 발생하도록 하고, 정상 모드로의 복귀는 더 낮은 임계값 아래에서 일정 시간 동안 안정적인 상태가 유지된 이후 허용할 수 있다. 이를 통해 빠른 모드 진동을 방지하고 큐, 온도, 자원 압력이 안정적인 상태로 돌아올 충분한 시간을 확보할 수 있다.

복구(recovery)는 성능 저하만큼 중요하다. 과부하가 사라지면 모든 워크로드를 한 번에 복원하는 대신 점진적으로 복원해야 한다. 시스템은 먼저 중요한 마감시간과 정보 최신성이 안정화되었는지 확인한 다음 인지 품질, 예측 범위, 지도 작성, 고수준 추론(high-level reasoning)을 단계적으로 복원할 수 있다. 중지했던 모든 작업을 즉시 다시 시작하면 원래의 과부하가 다시 발생할 수 있다. 따라서 점진적 복구(graceful recovery)는 자원 인식 복원 순서(resource-aware sequencing)와 지속적인 컴퓨팅 여유(computational headroom)의 확인을 요구한다.

열 및 전력 제약조건(thermal and power constraints)은 명목상의 하드웨어 구성이 충분해 보이는 경우에도 과부하를 발생시킬 수 있다. 지속적인 GPU 계산은 열 스로틀링을 발생시켜 클록 주파수(clock frequency)를 낮추고 추론 지연시간을 증가시킬 수 있다. 배터리 기반 로봇은 모든 가속기를 동시에 최고 성능으로 동작시키지 못하도록 전력 제한을 적용할 수도 있다. 따라서 컴퓨팅 스케줄링(compute scheduling)은 온도, 전력 소비(power consumption), 배터리 상태(battery state), 냉각 능력(cooling capacity)을 사용 가능한 실시간 컴퓨팅 예산(real-time computational budget)의 일부로 고려해야 한다.

다중 로봇 시스템(multi-robot system)은 계산 수요를 다른 자원으로 재분배할 수 있다는 또 다른 가능성을 제공한다. 로컬 과부하가 발생한 로봇은 통신 상태가 허용되는 경우 중요하지 않은 처리를 온프레미스 서버(on-premise server) 또는 주변 컴퓨팅 노드로 오프로딩(offloading)할 수 있다. 그러나 오프로딩은 네트워크 지연시간과 연결성(connectivity)에 대한 의존성을 추가한다. 안전 중요 제어(safety-critical control)는 로컬에 유지해야 하며 원격 자원은 선택적인 계산 가속 기능을 제공해야 한다. 따라서 원격 자원을 사용할 수 없게 되더라도 안전한 운용 능력이 사라지는 것이 아니라 기능 수준만 감소해야 한다.

점진적 성능 저하 정책(graceful degradation policy)은 의도적으로 과부하를 발생시키는 조건에서 검증해야 한다. 시험에서는 밀집된 센서 장면, 최대 카메라 트래픽, 복잡한 계획 상황, 동시 인공지능 추론, 로깅, 네트워크 부하, 열 스트레스(thermal stress), 감소된 컴퓨팅 가용성을 조합할 수 있다. 엔지니어는 지연시간 분포(latency distribution), 큐 깊이, 정보 연령, 마감시간 위반, 제어 안정성(control stability), 모드 전환(mode transition), 정지 동작(stopping behavior)을 측정해야 한다. 목적은 자원 고갈(resource exhaustion)이 예측할 수 없는 물리적 동작이 아니라 예측 가능한 기능 축소로 이어지는지를 검증하는 것이다.

궁극적으로 점진적 성능 저하(graceful degradation)는 컴퓨팅 과부하를 통제되지 않는 장애 상태에서 관리 가능한 운용 상태(managed operating state)로 전환한다. 물리 인공지능은 계산 능력에 대한 요구 수준을 사용 가능한 자원, 작업 긴급도(task urgency), 환경 위험(environmental risk), 물리적 동역학에 지속적으로 맞추어야 한다. 자원이 감소하면 의미론적 풍부함(semantic richness), 예측 깊이(prediction depth), 모델 복잡도(model complexity), 계획 품질(planning quality)을 점진적으로 낮추면서 결정론적 제어(deterministic control)와 안전 기능은 보호해야 한다. 물리 시스템이 안전하지 않은 상태가 되기 전에 지능이 먼저 더 단순하고, 더 국소적이며, 더 보수적인 형태로 변화해야 한다.

## 06.12. Latency Budgeting and Profiling [w/Code]

![](images/image14.png){width="7.268055555555556in" height="7.268055555555556in"}

지연시간 예산 수립 및 프로파일링(latency budgeting and profiling)은 물리 인공지능(Physical AI) 시스템이 안전한 물리적 상호작용에 허용된 시간 안에 센싱(sensing), 인지(perception), 추론(reasoning), 계획(planning), 제어(control), 액추에이션(actuation)을 완료할 수 있는지를 판단하기 위한 정량적 기반을 제공한다. 이 활동은 지연시간 및 실시간 제약조건(latency and real-time constraints)을 실제 구현 수준에서 검증하고 최적화하는 과정으로 볼 수 있으며, 시스템 수준의 타이밍 요구사항을 측정 가능한 공학적 지표로 변환한다.

지연시간 예산 수립(latency budgeting)은 물리적 요구사항으로부터 최대 허용 센싱-투-액션(sensing-to-action) 응답시간을 정의하는 것에서 시작한다. 로봇 속도, 정지 거리(stopping distance), 액추에이터 동역학(actuator dynamics), 제어 주파수(control frequency), 환경 불확실성(environmental uncertainty), 안전 여유(safety margin)가 시스템이 얼마나 빠르게 반응해야 하는지를 결정한다. 이렇게 도출된 마감시간(deadline)은 센싱, 통신, 전처리(preprocessing), 추론, 융합(fusion), 예측, 계획, 명령 생성(command generation), 액추에이터 응답에 분배해야 하는 유한한 자원이 된다.

유용한 지연시간 예산(latency budget)은 전체 파이프라인을 측정 가능한 단계로 분해한다. 센서 노출(sensor exposure)이나 스캐닝(scanning)은 데이터 전송이 시작되기 전부터 시간을 소비하고, 통신은 전송 지연을 발생시키며, 버퍼링(buffering)은 정보 연령(information age)을 증가시킬 수 있다. 전처리는 추론을 위해 데이터를 준비하고, 인공지능 모델은 계산시간을 소비하며, 예측 또는 계획은 명령이 액추에이터에 도달하기 전에 의사결정을 생성한다. 따라서 전체 센싱-투-액션 경로를 분석해야 하며 인공지능 추론시간만을 측정해서는 충분하지 않다.

전체 예산은 단계별 지연시간(stage latency)에 동기화(synchronization)와 스케줄링(scheduling)의 영향을 더한 형태로 표현할 수 있다. 그러나 각 구성요소에 평균 실행시간만 할당하는 것은 충분하지 않다. 물리 인공지능 워크로드는 장면 복잡도(scene complexity), 자원 경합(resource contention), 메모리 동작, 네트워크 트래픽, 열 조건(thermal condition), 동시 실행(concurrent execution)에 따라 변동한다. 따라서 예산에는 이상적인 정상 상태 실행만 가정하기보다 최대 또는 높은 백분위 동작(high-percentile behavior)과 예비 여유(reserve margin)를 포함해야 한다.

프로파일링(profiling)은 실제 배포 하드웨어에서 시간이 어디에서 소비되는지를 측정하는 과정이다. 이를 통해 아키텍처상의 가정을 경험적 증거(empirical evidence)로 변환할 수 있다. 프로파일링에서는 센서 획득(sensor acquisition), 데이터 도착, 전처리 시작과 종료, 추론 경계(inference boundary), 융합, 계획, 명령 발행(command publication), 제어기 수신(controller reception), 액추에이터 응답에 타임스탬프(timestamp)를 기록해야 한다. 이러한 단계별 계측(stage-level instrumentation)을 통해 실제 임계 경로(critical path)를 재구성하고 미들웨어, 큐(queue), 데이터 전송, 동기화 내부에 숨겨진 지연을 찾아낼 수 있다.

프로파일링은 최적화를 시작하기 전에 기준 구성(baseline configuration)을 설정하는 것에서 출발해야 한다. 기준선(baseline)은 의도된 모델 정확도와 시스템 동작을 유지하면서 종단간 지연시간(end-to-end latency)과 개별 파이프라인 단계의 기여도를 기록한다. 이후 추정된 병목이 아니라 실제 측정된 병목(bottleneck)을 대상으로 최적화를 수행할 수 있다. 순차 실행(sequential execution), 메모리 복사(memory copy), 전처리, 동기화, 모델 실행 등이 주요 최적화 대상이 될 수 있으며, 최적화 전후의 지연시간을 동일한 조건에서 비교해야 한다.

평균 지연시간(average latency)은 유용하지만 그 자체만으로 실시간 신뢰성(real-time reliability)을 설명할 수 없다. 프로파일링에서는 중앙값(median), P95, P99, 최대 관측 지연시간(maximum observed latency), 지터(jitter), 마감시간 위반(deadline miss)을 포함한 분포를 측정해야 한다. 평균값은 낮지만 간헐적으로 극단적인 지연을 발생시키는 파이프라인은 조금 느리더라도 실행시간이 엄격하게 제한되는 파이프라인보다 실시간 시스템에 부적합할 수 있다. 따라서 일반적인 성능뿐만 아니라 현실적인 워크로드에서의 긴 꼬리 동작(long-tail behavior)까지 확인해야 한다.

정보 연령은 처리 지연시간(processing latency)과 함께 측정해야 한다. 신경망 자체의 추론시간은 짧더라도 처리되는 프레임이 이미 큐에서 상당한 시간을 기다렸다면 실제 시스템은 현재 환경에 빠르게 대응한다고 볼 수 없다. 모델 또는 커널(kernel)의 실행시간만 측정하면 시스템이 충분히 반응성이 있는 것처럼 잘못 판단할 수 있다. 따라서 큐 깊이(queue depth), 프레임 연령(frame age), 드롭된 프레임(dropped frame), 마감시간 위반은 실시간 엣지 추론(real-time edge inference)과 센싱-투-액션 성능을 평가하는 중요한 관측 지표(observability signal)이다.

CPU와 가속기 프로파일링(accelerator profiling)은 사용률(utilization), 동시성(concurrency), 동기화, 유휴 구간(idle period)을 보여주어야 한다. 높은 GPU 사용률이 반드시 효율적인 실시간 파이프라인을 의미하는 것은 아니다. 가속기가 이미 오래된 큐 데이터를 처리하고 있거나 다른 중요 워크로드를 차단하고 있을 수도 있다. 반대로 낮은 사용률은 CPU 전처리, 데이터 전송, 동기화 장벽(synchronization barrier), 직렬화된 실행(serialized execution) 때문에 발생하는 파이프라인 버블(pipeline bubble)을 의미할 수 있다. 따라서 사용률 자체가 아니라 종단간 지연시간과의 관계를 분석해야 한다.

메모리 프로파일링(memory profiling) 역시 중요하다. 산술 계산이 효율적이더라도 데이터 이동(data movement)이 전체 실행시간을 지배할 수 있기 때문이다. CPU-GPU 복사, 중간 특징 맵(intermediate feature map), 텐서 할당(tensor allocation), 캐시 동작(cache behavior), 메모리 대역폭(memory bandwidth), 반복적인 데이터 형식 변환(format conversion)은 상당한 지연을 추가할 수 있다. 제로카피 버퍼(zero-copy buffer), 메모리 풀링(memory pooling), 텐서 재사용(tensor reuse), 개선된 데이터 레이아웃(data layout), 저정밀도 연산(reduced precision), 불필요한 데이터 전송 제거 등을 전체 파이프라인에 대한 실제 기여도를 기준으로 평가해야 한다.

통신 프로파일링(communication profiling)은 측정 경계를 하나의 프로세서 밖으로 확장한다. 센서 인터페이스, Ethernet 링크, 스위치, 미들웨어, 프로세스 간 통신(inter-process communication), 원격 컴퓨팅 경로(remote compute path)에 타임스탬프를 기록하여 단방향 지연(one-way delay), 큐잉(queueing), 지터, 패킷 손실(packet loss)을 구분할 수 있어야 한다. 온프레미스(on-premise) 또는 클라우드 계산을 사용하는 경우 전체 왕복 경로(round-trip path)를 포함해야 한다. 원격 모델의 실행 자체가 빠르더라도 네트워크와 서버 지연이 지배적이라면 물리적 마감시간을 위반할 수 있다.

동시성은 지연시간 동작을 크게 변화시킨다. 인지, 월드 모델링(world modeling), 계획, 지도 작성(mapping), 로깅(logging), 시각화(visualization)가 각각 독립적으로 시험될 때에는 예산을 만족하더라도 동시에 실행되면 서로 간섭할 수 있다. GPU 커널은 가속기 자원을 놓고 경쟁할 수 있고 CPU 스레드는 서로를 선점(preemption)할 수 있으며 공유 메모리나 입출력(I/O)이 포화될 수 있다. 따라서 프로파일링은 개별 모듈 시험에서 시작하여 실제 배포 시스템을 재현하는 통합 동시 워크로드(integrated concurrent workload)로 확장되어야 한다.

예산 테이블(budget table)은 이러한 제약조건을 실용적인 공학 형태로 표현한다. 각 파이프라인 또는 센서에 대해 엔지니어는 원시 및 처리 데이터율(raw and processed data rate), 메모리 요구량, 컴퓨팅 요구량(compute demand), 지연시간 할당(latency allocation), 전력 소비(power consumption), 최대 사용률(peak utilization)을 기록할 수 있다. 임베디드 시스템에서는 지연시간을 대역폭, 메모리, 전력, 열 용량(thermal capacity)과 완전히 분리하여 다룰 수 없기 때문에 이러한 항목을 통합적으로 관리해야 한다.

프로파일링에서는 파이프라인 중첩(pipeline overlap)도 확인해야 한다. 센서 획득, 전처리, 추론, 계획, 제어가 반드시 순차적으로 실행되는 것은 아니다. 비동기 실행(asynchronous execution)을 사용하면 CPU 전처리와 GPU 추론을 중첩하거나 서로 다른 센서 프레임을 동시에 처리할 수 있다. 따라서 중요한 지표는 독립적으로 측정한 함수 실행시간의 단순한 합이 아니라 관측에서 실제 물리적 응답까지 이어지는 임계 경로 지연시간(critical-path latency)이다. 최적화는 개별 함수의 시간 합보다 이 임계 경로를 단축하는 방향으로 이루어져야 한다.

프로파일링을 통해 병목이 확인되면 여러 수준에서 최적화를 수행할 수 있다. 모델에서는 양자화(quantization), 가지치기(pruning), 효율적인 백본(efficient backbone), 계층 융합(layer fusion), 지식 증류(knowledge distillation)를 적용할 수 있다. 파이프라인에서는 중복 연산을 제거하고 비동기적으로 실행하며 유용한 동시성을 높일 수 있다. 메모리 경로에서는 복사를 줄이고 버퍼를 재사용할 수 있으며, 소프트웨어 런타임(runtime)은 그래프, 연산자(operator), 커널을 최적화할 수 있다. 시스템 수준에서는 센서 해상도, 관심 영역(region of interest), 실행 주기 또는 스케줄링을 조정할 수 있다.

최적화 이후에는 반드시 다시 프로파일링해야 한다. 하나의 단계를 개선하면 병목이 다른 곳으로 이동할 수 있기 때문이다. 추론을 가속하면 전처리가 지배적인 지연요소가 될 수 있고, 전처리를 가속하면 메모리 전송 또는 계획 지연시간이 새로운 병목으로 나타날 수 있다. 따라서 지연시간 엔지니어링(latency engineering)은 측정, 임계 경로 식별, 최적화, 재측정, 예산 갱신을 반복하는 과정이다. 전체 배포 파이프라인이 충분한 타이밍 여유를 가지고 요구사항을 만족할 때까지 하드웨어-소프트웨어 공동 설계(hardware-software co-design)를 반복해야 한다.

예비 여유(reserve margin)는 정상 조건에서 측정된 성능이 미래의 타이밍을 보장하지 않기 때문에 필요하다. 예산에는 순간적으로 증가하는 센서 트래픽, 복잡한 장면, 운영체제 간섭, 네트워크 변동, 열 스로틀링(thermal throttling), 동시 워크로드를 처리할 수 있는 여유 용량을 남겨야 한다. 정상 운용에서도 마감시간 대부분을 소비하는 설계는 회복탄력성(resilience)이 낮다. 따라서 평균적인 자원 사용량만이 아니라 최대 동작(peak behavior), 변동성(variance), 예비 여유를 함께 고려해야 한다.

열 및 전력 프로파일링(thermal and power profiling)은 지속적인 운용 상태에서 지연시간 측정과 함께 수행해야 한다. GPU가 짧은 벤치마크에서는 지연시간 목표를 만족하더라도 온도가 상승하여 주파수 스로틀링(frequency throttling)이 발생하면 이후에는 더 느려질 수 있다. 마찬가지로 배터리 또는 플랫폼 전력 제한 때문에 최대 컴퓨팅 성능을 지속적으로 사용할 수 없을 수도 있다. 따라서 지속 성능(sustained performance)은 전력, 열 한계, 냉각 능력(cooling capacity), 스로틀링 회피와 함께 현실적인 주변 환경 조건에서 검증해야 한다.

프로파일링 도구(profiling tool)와 계측 장치(instrumentation)는 측정 대상의 타이밍 자체를 변화시킬 수 있으므로 신중하게 사용해야 한다. 가벼운 타임스탬프와 하드웨어 카운터(hardware counter)를 이용하면 지속적인 관측 가능성(observability)을 제공할 수 있으며, 보다 상세한 추적(trace)은 전용 진단 실행에서 활성화할 수 있다. 이벤트가 여러 프로세서 또는 네트워크 노드 사이를 이동하는 경우 동기화된 클록(synchronized clock)을 사용해야 한다. 그렇지 않으면 타임스탬프 차이가 실제 통신 또는 실행 지연이 아니라 클록 오차(clock error)를 나타낼 수 있다.

자동화된 프로파일링(automated profiling)은 런타임 추적(runtime trace)을 단계별 통계와 예산 보고서로 변환할 수 있다. 소프트웨어 구현에서는 각 프레임 또는 제어 주기의 타임스탬프를 수집하고, 단계별 실행시간과 종단간 지연시간을 계산하고, 백분위 분포(percentile distribution)를 추정하고, 마감시간 위반을 탐지하며, 임계 경로에서 가장 큰 지연 기여 요소를 식별할 수 있다. 이러한 계측은 아키텍처 수준의 지연시간 예산을 실제 실행 가능한 측정 및 검증 워크플로(measurement and validation workflow)와 연결한다.

검증(validation)은 유휴 상태의 로봇만 프로파일링하는 것이 아니라 어려운 운용 조건을 재현해야 한다. 전체 센서 트래픽, 동시 인공지능 추론, 지도 작성, 로깅, 네트워킹(networking), 저장장치 활동(storage activity), 열 스트레스(thermal stress), 백그라운드 워크로드를 동시에 활성화해야 한다. 이러한 실제 부하와 장시간 운용 조건에서 지연시간 분포, 가속기 사용률, 메모리 대역폭, 큐 깊이, 온도, 전력, 프레임 드롭(frame drop), 마감시간 위반을 측정해야 한다.

지연시간 예산 수립 및 프로파일링의 최종 결과는 단순히 더 빠른 벤치마크를 만드는 것이 아니다. 이는 실제 배포된 물리 인공지능 시스템이 충분히 최신인 관측(fresh observation)을 정의된 타이밍 제약조건 안에서 안전한 물리적 반응으로 변환할 수 있다는 증거를 확보하는 과정이다. 따라서 효과적인 엔지니어링은 요구사항 정의, 예산 할당, 대상 하드웨어 프로파일링(target-hardware profiling), 병목 식별, 최적화, 스트레스 검증, 런타임 모니터링(runtime monitoring)을 지속적으로 반복하여 실시간 능력이 개별 인공지능 처리량이 아니라 전체 물리 시스템 수준에서 측정되고 보장되도록 해야 한다.
