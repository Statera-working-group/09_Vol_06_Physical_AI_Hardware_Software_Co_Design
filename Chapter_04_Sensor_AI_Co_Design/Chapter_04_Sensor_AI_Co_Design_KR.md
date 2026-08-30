**Volume 06. Physical AI Hardware Software Co Design**

# Chapter 04. Sensor AI Co Design

## 04.01. Sensing as an AI System Design Problem

![](images/image1.png){width="7.268055555555556in" height="7.268055555555556in"}

![](images/image2.png){width="7.268055555555556in" height="7.268055555555556in"}

피지컬 AI(Physical AI)에서 센싱(Sensing)은 AI 개발 이전에 수행되는 별도의 하드웨어 선택 활동으로 다루어서는 안 됩니다. 센싱은 근본적으로 AI 시스템 설계(AI System Design) 문제입니다. 로봇이 무엇을 인식할 수 있는지에 관한 모든 결정은 내부 세계 표현(World Representation)에 어떤 정보가 들어올 수 있는지를 결정하기 때문입니다. 따라서 센서 선택은 인지(Perception), 월드 모델링(World Modeling), 추론(Reasoning), 계획(Planning), 제어(Control)가 작동할 수 있는 관측 가능 영역(Observable Boundary)을 설정합니다.

피지컬 AI 시스템(Physical AI System)은 물리 세계(Physical World)를 직접 관측하지 않습니다. 대신 카메라(Camera), 라이다(LiDAR), 레이더(Radar), 관성 센서(Inertial Sensor), 엔코더(Encoder), 힘 센서(Force Sensor), 마이크(Microphone) 등의 장치가 물리적 현상을 샘플링된 측정값(Sampled Measurement)으로 변환합니다. AI 시스템은 이러한 측정값으로부터 객체(Object), 기하 구조(Geometry), 움직임(Motion), 접촉(Contact), 지형(Terrain), 로봇 상태(Robot State)를 추정합니다. 따라서 센싱 단계에서 발생한 한계는 이후의 모든 지능 처리 단계로 전파됩니다.

이러한 관점은 기존의 "로봇에 어떤 센서를 장착해야 하는가?"라는 질문을 "AI가 임무를 신뢰성 있게 수행하기 위해 어떤 정보를 획득해야 하는가?"라는 질문으로 변화시킵니다. 창고 자율이동로봇(AMR), 실외 배송 로봇(Outdoor Delivery Robot), 4족 보행 로봇(Quadruped), 휴머노이드(Humanoid), 자율주행차(Autonomous Vehicle)는 모두 위치 추정(Localization)과 장애물 회피(Obstacle Avoidance)가 필요할 수 있지만, 관측 환경과 운동 동역학(Motion Dynamics), 고장 결과(Failure Consequence), 상호작용 요구사항은 크게 다릅니다. 따라서 센서 아키텍처(Sensor Architecture)는 임무 및 환경 요구사항에서 출발해야 합니다.

센싱 문제는 관측 가능성 문제(Observability Problem)로 이해할 수 있습니다. 물리적 환경과 로봇을 기본 상태(Underlying State) (x_t)로 표현하고 센서가 관측값(Observation) (y_t)를 생성한다고 하면, 센싱은 개념적으로 (y_t=h(x_t)+n_t)라는 매핑(Mapping)을 형성합니다. 여기에서 (h(\\cdot))는 측정 과정(Measurement Process)을, (n_t)는 불확실성(Uncertainty)과 잡음(Noise)을 나타냅니다. AI는 불완전하고 잡음이 있으며 지연되거나 때로는 서로 모순되는 관측값으로부터 (x_t)의 유용한 속성을 추론해야 합니다.

더 많은 센싱(More Sensing)이 자동으로 더 높은 지능을 의미하지는 않습니다. 카메라 수를 늘리면 공간 커버리지(Spatial Coverage)는 증가하지만 이미지 대역폭(Image Bandwidth), 메모리 트래픽(Memory Traffic), 전처리 부하(Preprocessing Load), GPU 사용률(GPU Utilization), 전력 소비(Power Consumption), 열 부하(Thermal Load), 동기화 복잡도(Synchronization Complexity), 캘리브레이션 작업(Calibration Effort)도 함께 증가합니다. 따라서 컴퓨팅 시스템이 요구 지연시간 내에서 추가 정보를 처리하지 못한다면 고해상도 센싱은 오히려 전체 시스템 성능을 저하시킬 수 있습니다.

올바른 센싱 아키텍처(Sensing Architecture)는 원시 센서 사양(Raw Sensor Specification)이 아니라 정보 가치(Information Value)를 기준으로 결정되어야 합니다. 해상도(Resolution), 거리(Range), 시야각(Field of View), 동적 범위(Dynamic Range), 갱신 주기(Update Frequency), 측정 불확실성(Measurement Uncertainty), 환경 강건성(Environmental Robustness), 고장 특성(Failure Characteristics)은 후속 AI 작업에 얼마나 기여하는지를 기준으로 평가해야 합니다. 막대한 데이터를 생성하는 센서보다 중요한 물리 상태를 직접 제한할 수 있는 저대역폭 센서가 더 높은 가치를 제공할 수도 있습니다.

서로 다른 센서 모달리티(Sensor Modality)는 현실의 서로 다른 측면을 관측합니다. 카메라(Camera)는 풍부한 외형, 질감, 색상, 의미 정보를 제공하고, 라이다(LiDAR)는 직접적인 기하학적 측정값을 제공합니다. 레이더(Radar)는 열악한 환경에서도 유용한 거리 및 속도 정보를 제공하며, 관성측정장치(IMU)는 빠른 자기 운동(Ego-Motion)을 측정합니다. 엔코더(Encoder)는 액추에이터와 관절 상태를 제공하고, 힘 또는 토크 센서(Force or Torque Sensor)는 물리적 상호작용을 관측합니다. 다중모달 센싱(Multimodal Sensing)이 필요한 이유는 단일 모달리티가 피지컬 AI에 필요한 모든 상태를 완전하게 관측할 수 없기 때문입니다.

센서 배치(Sensor Placement) 역시 AI 설계의 일부입니다. 이론적으로 성능이 뛰어난 센서라도 시야가 가려지거나 중요한 영역을 볼 수 없고, 진동이 측정값을 왜곡하거나 장착 구조로 인해 지속적인 사각지대(Blind Zone)가 발생한다면 가치가 크게 감소합니다. 로봇의 체화 구조(Embodiment)는 센서를 물리적으로 어디에 배치할 수 있는지를 결정합니다. 따라서 인지 성능은 센서 성능뿐 아니라 센서 배치, 로봇 형태(Morphology), 캘리브레이션(Calibration), 환경 기하 구조(Environmental Geometry), AI 모델의 결합에 의해 결정됩니다.

시간적 동작(Temporal Behavior)도 근본적인 제약조건입니다. 피지컬 AI는 로봇과 환경이 지속적으로 변화하는 상황에서 동작합니다. 서로 다른 주기로 측정하거나 타임스탬프(Timestamp)가 일치하지 않는 센서 데이터는 소프트웨어에서 동시에 처리되더라도 서로 다른 물리적 순간을 나타낼 수 있습니다. 따라서 수십 FPS의 카메라, 고주파 IMU, 라이다 스캔(LiDAR Scan), 휠 엔코더(Wheel Encoder), 비동기 이벤트 스트림(Asynchronous Event Stream)을 함께 사용하려면 명시적인 동기화(Synchronization), 버퍼링(Buffering), 타임스탬핑(Timestamping), 시간 정렬(Temporal Alignment)이 필요합니다.

센싱은 센서의 명목상 주파수(Nominal Sensor Frequency)가 아니라 종단간 지연시간(End-to-End Latency)을 기준으로 설계해야 합니다. 카메라가 빠른 속도로 프레임을 생성하더라도 노출(Exposure), 데이터 전송(Transfer), 디코딩(Decoding), 전처리(Preprocessing), 신경망 추론(Neural Inference), 센서 융합(Sensor Fusion), 스케줄링(Scheduling)을 거치면서 실제 행동에 반영되기까지 상당한 지연이 발생할 수 있습니다. 이동하는 로봇에서 지연된 인지는 이미 존재하지 않는 과거의 세계를 설명하는 것과 같습니다. 따라서 중요한 지표는 AI가 의사결정을 내리는 순간 정보가 얼마나 최신이며 신뢰할 수 있는가입니다.

센싱과 컴퓨팅(Compute)의 관계는 중요한 공동설계 루프(Co-Design Loop)를 형성합니다. 이미지 해상도를 높이면 원거리 객체 인식 성능이 향상될 수 있지만 더 큰 신경망 백본(Neural Backbone)이나 추가적인 AI 가속기(AI Accelerator)가 필요할 수 있습니다. 라이다 밀도(LiDAR Density)를 높이면 기하학적 표현은 개선되지만 포인트 클라우드(Point Cloud) 처리량과 메모리 요구량이 증가합니다. 센서 갱신율을 높이면 시간적 불확실성은 감소하지만 통신 및 컴퓨팅 예산을 더 많이 소비합니다. 따라서 모든 센싱 성능 향상은 전체 시스템 비용과 함께 평가되어야 합니다.

AI 아키텍처(AI Architecture)는 필요한 센싱 하드웨어 자체를 변화시킬 수도 있습니다. 강력한 시간 융합(Temporal Fusion) 능력을 가진 모델은 고가의 고해상도 센서 하나에 전적으로 의존하기보다 여러 개의 저비용 관측값으로부터 유용한 정보를 추출할 수 있습니다. 학습 기반 다중모달 표현(Learned Multimodal Representation)은 개별 모달리티의 약점을 보완할 수 있으며, 월드 모델(World Model)은 관측이 일시적으로 사라지더라도 잠재 상태(Latent State)를 유지하고 추정할 수 있습니다. 따라서 센서 설계와 모델 설계는 순차적으로 수행하기보다 함께 발전시켜야 합니다.

그러나 이러한 상호작용이 AI가 물리적 센싱 한계(Physical Sensing Limitation)를 제거할 수 있다는 의미는 아닙니다. 애초에 측정되지 않은 정보는 특히 새로운 환경이나 안전 중요 상황(Safety-Critical Situation)에서 항상 신뢰성 있게 복원할 수 있는 것이 아닙니다. 학습 기반 추론(Learning-Based Inference)은 상관관계를 이용하여 숨겨진 변수를 추정할 수 있지만 이러한 추정에는 불확실성이 존재하며 학습 분포(Training Distribution)를 벗어나면 실패할 수 있습니다. 따라서 하드웨어 관측 가능성(Hardware Observability)과 학습 기반 추론은 서로를 대체하기보다 상호 보완해야 합니다.

중복성(Redundancy)은 또 다른 시스템 수준의 고려사항입니다. 여러 센서가 중첩된 증거를 제공하면 어둠, 눈부심, 먼지, 비, 진동, 가림(Occlusion), 오염, 통신 오류, 하드웨어 고장 등으로 하나의 모달리티가 성능 저하 상태에 들어가더라도 시스템 기능을 유지할 수 있습니다. 그러나 중복성은 전력, 대역폭, 컴퓨팅 자원, 물리적 공간, 비용을 소비합니다. 따라서 유용한 중복 설계는 단순히 동일한 측정을 복제하는 것이 아니라 서로 다른 고장 모드(Failure Mode)를 보완하도록 구성해야 합니다.

피지컬 AI 센싱에는 외부 환경의 인지뿐 아니라 고유수용감각(Proprioception)도 포함되어야 합니다. 관절 위치(Joint Position), 휠 속도(Wheel Speed), 모터 전류(Motor Current), 배터리 상태(Battery State), 온도(Temperature), 힘(Force), 토크(Torque), 가속도(Acceleration) 등의 내부 측정값은 로봇 자체의 물리적 상태를 설명합니다. 이러한 신호를 통해 AI는 환경 변화와 자신의 신체 상태 변화를 구분할 수 있으며, 제어(Control), 접촉 추정(Contact Estimation), 이상 탐지(Anomaly Detection), 예지 정비(Predictive Maintenance), 적응 행동(Adaptive Behavior)을 수행할 수 있습니다.

센싱 파이프라인(Sensing Pipeline)은 학습 및 운용 데이터의 구조도 결정합니다. 센서 캘리브레이션, 좌표계(Coordinate System), 타임스탬프, 누락 측정값(Missing Measurement), 잡음 특성(Noise Characteristics), 압축(Compression), 전처리 과정은 학습에 사용되는 데이터셋(Dataset)에 그대로 반영됩니다. 따라서 특정 센싱 구성으로 학습된 모델은 센서 위치, 펌웨어(Firmware), 해상도, 노출 조건, 동기화 방식이 변경되면 성능이 저하될 수 있습니다. 기본 임무가 동일하더라도 하드웨어 변경 자체가 AI 모델에는 분포 변화(Distribution Shift)가 될 수 있습니다.

강건한 설계 과정(Robust Design Process)에서는 각각의 센싱 요구사항을 후속 시스템 결정까지 추적할 수 있어야 합니다. 필요한 세계 상태(World State)가 요구 관측값을 결정하고, 관측값이 센서 모달리티와 배치를 결정하며, 센서 구성이 대역폭과 동기화 요구사항을 결정합니다. 이것이 다시 컴퓨팅 및 메모리 워크로드(Workload)를 결정하고, 컴퓨팅 제약이 모델 아키텍처와 추론 주기(Inference Rate)에 영향을 미칩니다. 최종적인 모델 성능은 다시 최초 센싱 구성이 충분했는지를 판단하게 하므로 전체 과정은 단방향이 아니라 반복적인 공동설계 과정입니다.

따라서 이 장의 구조는 센싱 문제(Sensing Problem)에서 시작하여 모달리티 선택(Modality Selection), 센서 특성, 배치, 공간 커버리지, 중복성, 해상도, 갱신율, 대역폭, 시간 동기화, 센서 융합, 모델 기반 센서 선택(Model-Driven Sensor Selection), 센서 성능 저하 상태에서의 운용(Degraded Operation), 정량적 자원 예산화(Quantitative Budgeting)로 발전하도록 구성됩니다. 이러한 요소들은 서로 독립적인 센서 엔지니어링 문제가 아니라 하나의 피지컬 AI 아키텍처를 구성하는 상호 연결된 설계 차원입니다.

궁극적인 목표는 센서의 수나 개별 센서의 성능을 최대화하는 것이 아니라 물리 세계와 인공지능 사이에 최소 충분성(Minimum Sufficiency)과 적절한 중복성을 갖춘 정보 인터페이스(Information Interface)를 구축하는 것입니다. 효과적인 센싱은 임무에 필요한 상태를 허용 가능한 불확실성, 지연시간, 전력, 컴퓨팅 자원, 대역폭, 비용, 고장 허용성(Fault Tolerance) 범위 안에서 관측 가능하게 만듭니다. 피지컬 AI에서 센싱은 지능 아키텍처(Intelligence Architecture) 자체의 일부이며, 기계가 무엇을 지능적으로 판단할 수 있는가는 결국 시스템이 무엇을 물리적으로 관측하도록 설계되었는가에서 시작됩니다.

## 04.02. Sensor Modality Selection

![](images/image3.png){width="7.268055555555556in" height="7.268055555555556in"}

![](images/image4.png){width="7.268055555555556in" height="7.268055555555556in"}

피지컬 AI(Physical AI)에서 센서 모달리티 선택(Sensor Modality Selection)은 AI 시스템이 환경을 신뢰성 있게 이해하고, 자신의 상태를 추정하며, 적절한 행동을 실행하기 위해 어떤 물리적 현상을 측정해야 하는지를 결정하는 과정입니다. 따라서 이는 단순히 센서 사양(Sensor Specification)을 비교하는 문제가 아닙니다. 센서 선택은 임무(Task), 환경(Environment), 체화(Embodiment), 필요한 세계 상태(World State), 그리고 허용 가능한 불확실성(Uncertainty), 지연시간(Latency), 비용(Cost), 고장 위험(Failure Risk)에서 출발해야 합니다.

각 센싱 모달리티(Sensing Modality)는 물리 세계(Physical World)의 서로 다른 투영(Projection)을 제공합니다. 카메라(Camera)는 반사광을 측정하여 외형, 질감, 색상, 객체 정체성(Object Identity), 의미적 맥락(Semantic Context)에 대한 밀도 높은 정보를 제공합니다. 라이다(LiDAR)는 기하학적 거리(Geometric Range)를 직접 측정하여 공간적으로 구조화된 포인트 클라우드(Point Cloud)를 생성하며, 레이더(Radar)는 무선주파수 신호(Radio-Frequency Signal)를 이용하여 거리와 상대 속도(Relative Velocity)를 측정합니다. 이러한 모달리티는 동일한 정보를 서로 다른 방식으로 제공하는 것이 아니라 상호 보완적인 물리적 속성을 관측합니다.

관성측정장치(Inertial Measurement Unit, IMU)는 비교적 높은 갱신율(Update Rate)로 가속도(Acceleration)와 각속도(Angular Velocity)를 제공하므로 느린 환경 센서의 측정 사이에서 발생하는 빠른 움직임을 추정하는 데 중요합니다. 휠 엔코더(Wheel Encoder)와 관절 엔코더(Joint Encoder)는 로봇 자체의 움직임에 대한 직접적인 정보를 제공합니다. 힘(Force), 토크(Torque), 촉각(Tactile), 모터 전류(Motor Current) 등의 고유수용감각(Proprioceptive) 측정값은 지능이 원격 관측뿐 아니라 물리적 접촉에 의존할 때 특히 중요해집니다.

모달리티 선택의 첫 번째 기준은 임무 관련성(Task Relevance)입니다. 센서는 특정 인지(Perception), 월드 모델링(World Modeling), 계획(Planning), 제어(Control), 상호작용(Interaction), 안전(Safety) 기능에 필요한 정보를 제공하기 때문에 포함되어야 합니다. 객체 인식(Object Recognition)은 시각 정보가 중요할 수 있으며, 기하학적 내비게이션(Geometric Navigation)은 신뢰성 있는 공간 구조가 필요합니다. 조작(Manipulation)에서는 접촉 및 힘 정보가 중요해지고, 고동적 이동성(High-Dynamic Mobility)에서는 정확한 운동 추정(Motion Estimation)의 중요성이 증가합니다. 따라서 센서 요구사항은 임무에 필요한 정보에서 파생되어야 합니다.

환경 조건(Environmental Condition)은 두 번째 주요 선택 기준입니다. 조명(Illumination), 날씨(Weather), 먼지(Dust), 반사 표면(Reflective Surface), 진동(Vibration), 온도(Temperature), 전자기 간섭(Electromagnetic Interference), 지형 구조(Terrain Structure), 실내 또는 실외 운용, 예상 작동 거리 등은 특정 모달리티의 유용성을 크게 변화시킬 수 있습니다. 통제된 창고 환경을 위해 설계된 센싱 아키텍처(Sensing Architecture)가 어둠, 비, 안개, 식생(Vegetation), 불규칙 지형에 노출되는 실외 로봇에서도 동일한 관측 가능성(Observability)을 제공한다고 가정해서는 안 됩니다.

따라서 모달리티 선택에서는 상호 보완적인 고장 특성(Complementary Failure Characteristics)을 고려해야 합니다. 카메라는 매우 풍부한 의미 정보(Semantic Information)를 제공하지만 가시성(Visibility)과 조명에 크게 의존합니다. 라이다는 명시적인 기하 정보(Geometry)를 제공하지만 환경이나 표면 조건에 따라 성능이 저하될 수 있습니다. 레이더는 일반적으로 세밀한 공간 외형 정보는 부족하지만 광학 센싱(Optical Sensing)이 어려운 조건에서도 강건한 거리와 속도 정보를 제공할 수 있습니다. 여러 모달리티를 결합하면 하나의 물리적 측정 원리에 대한 의존성을 줄일 수 있습니다.

다중모달 센싱(Multimodal Sensing)의 목적은 사용 가능한 모든 센서를 설치하는 것이 아닙니다. 추가되는 모든 모달리티에는 시스템 비용(System Cost)이 발생합니다. 하드웨어 비용, 장착 요구사항, 캘리브레이션 파라미터(Calibration Parameter), 전력 소비, 통신 대역폭(Communication Bandwidth), 데이터 버퍼링(Data Buffering), 전처리(Preprocessing), 메모리 트래픽(Memory Traffic), 컴퓨팅 워크로드(Compute Workload), 동기화 요구사항, 추가적인 소프트웨어 복잡도가 발생합니다. 따라서 각 모달리티는 측정 가능한 정보 가치(Information Value), 강건성 향상 또는 필요한 중복성(Redundancy)을 통해 자신의 시스템 비용을 정당화할 수 있어야 합니다.

해상도(Resolution)와 거리(Range) 역시 독립적으로 최대화하기보다는 임무를 기준으로 평가해야 합니다. 고해상도 카메라는 세밀한 시각적 특징을 제공하고 고밀도 라이다(Dense LiDAR)는 기하학적 표현을 개선할 수 있지만, 두 경우 모두 데이터 이동량(Data Movement)과 처리 요구량이 증가합니다. 고속으로 이동하는 플랫폼에서는 AI가 인지하고 추론하며 계획하고 반응할 충분한 시간을 확보하기 위해 장거리 센싱(Long-Range Sensing)이 필수적일 수 있습니다. 반면 제한된 공간에서 저속으로 움직이는 로봇은 근본적으로 다른 센싱 거리 요구사항을 가질 수 있습니다.

갱신율(Update Rate)은 또 다른 중요한 절충 요소(Tradeoff)를 제공합니다. 빠르게 이동하는 로봇과 급격하게 변화하는 상호작용에서는 충분히 작은 시간 오차(Temporal Error)로 현재 물리 상태를 나타낼 수 있는 관측이 필요합니다. 고주파 IMU 또는 고유수용감각 측정은 낮은 주파수의 카메라나 라이다가 직접 표현하기 어려운 동역학(Dynamics)을 포착할 수 있습니다. 그러나 센서 주파수를 높이면 대역폭, 전처리, 동기화, 컴퓨팅 요구량도 증가하므로 갱신율은 추정하려는 상태의 동역학 특성에 맞추어야 합니다.

따라서 센서 모달리티 선택은 시간 융합(Temporal Fusion)과 연결되어야 합니다. 서로 다른 센서는 일반적으로 서로 다른 주파수로 작동하며 서로 다른 데이터 획득 및 통신 지연(Acquisition and Communication Delay)을 가집니다. 이러한 측정값을 단순히 동시 입력으로 취급해서는 안 됩니다. 다중모달 측정값이 일관된 세계 상태를 표현하려면 정확한 타임스탬프(Timestamp), 동기화(Synchronization), 버퍼링(Buffering), 보간(Interpolation), 운동 보상(Motion Compensation), 상태 추정(State Estimation)이 필요할 수 있습니다. 따라서 시간적 호환성(Temporal Compatibility)은 모달리티 호환성의 일부입니다.

체화(Embodiment)는 개별 모달리티의 가치도 변화시킵니다. 주로 이동을 통해 환경과 상호작용하는 휠 기반 자율이동로봇(AMR)은 접촉 중심 조립(Contact-Rich Assembly)을 수행하는 매니퓰레이터(Manipulator)와 서로 다른 센싱 요구사항을 가집니다. 4족 보행 로봇(Quadruped)은 신체 움직임, 지형, 발 디딤 위치(Foothold), 균형(Balance), 접촉 상태(Contact State)를 추론해야 하며, 휴머노이드(Humanoid)는 시각, 촉각, 힘, 관절, 관성 정보를 조정하여 사용할 필요가 있습니다. 기계의 물리적 형태가 무엇을 센싱해야 하는지와 센서를 어디에 배치할 수 있는지를 함께 결정합니다.

따라서 센서 배치(Sensor Placement)와 모달리티 선택은 함께 해결해야 합니다. 실제 사용 가능한 시야각(Field of View)을 결정하지 않고 카메라를 선택하는 것은 불완전하며, 로봇 본체에 의한 가림(Occlusion)을 고려하지 않고 라이다를 선택하는 것 역시 공간 커버리지(Spatial Coverage)를 비현실적으로 평가하게 만듭니다. 하나의 모달리티 자체가 근본적으로 부족해서가 아니라 로봇의 체화 구조가 사각 영역(Blind Region)을 만들기 때문에 여러 센서가 필요할 수도 있습니다. 물리적인 설치 기하 구조(Installation Geometry)는 AI가 실제로 사용할 수 있는 센싱 능력의 일부가 됩니다.

중복성(Redundancy)은 단순한 복제(Duplication)와 구분되어야 합니다. 비슷한 위치에 동일한 센서 두 개를 설치하면 개별 하드웨어 고장에는 대응할 수 있지만 동일한 환경 조건에 동시에 취약할 수 있습니다. 이종 중복성(Diverse Redundancy)은 서로 다른 센싱 원리를 이용하여 하나의 모달리티가 신뢰할 수 없게 되었을 때 다른 모달리티가 유용한 증거를 제공하도록 합니다. 필요한 아키텍처는 안전 요구사항(Safety Requirement), 성능 저하 확률, 고장 결과, 그리고 후속 AI가 불확실성을 인식할 수 있는 능력에 따라 달라집니다.

AI 모델 아키텍처(AI Model Architecture) 자체도 모달리티 선택에 영향을 줄 수 있습니다. 다중모달 신경망(Multimodal Neural Network)은 이미지, 포인트 클라우드, 레이더, 고유수용감각 등의 신호에서 공유 표현(Shared Representation)을 학습할 수 있습니다. 시간 모델(Temporal Model)은 여러 관측에 걸쳐 정보를 통합할 수 있으며, 월드 모델(World Model)은 부분적으로 관측된 상태의 추정값을 유지할 수 있습니다. 따라서 센서 선택에서는 각 장치가 독립적으로 무엇을 측정하는지만이 아니라 여러 측정값을 공간과 시간에 걸쳐 결합했을 때 AI가 어떤 정보를 추론할 수 있는지도 고려해야 합니다.

이것은 모델 주도 센서 선택(Model-Driven Sensor Selection) 문제를 형성합니다. 후보 센서 구성은 센서 사양만으로 평가하는 것이 아니라 후속 AI 성능(Downstream AI Performance)을 기준으로 평가할 수 있습니다. 특정 모달리티를 제거했을 때 대표적인 임무와 고장 조건에서 성능 저하가 거의 없다면 해당 센서의 시스템 비용은 정당화되지 않을 수 있습니다. 반대로 비교적 저렴한 센서라도 모호성(Ambiguity)을 해소하고, 상태 관측 가능성을 높이며, 독립적인 고장 대응 채널을 제공하거나 정상 운용 조건을 벗어난 상황에서 성능을 안정화한다면 매우 높은 가치를 가질 수 있습니다.

컴퓨팅 아키텍처(Compute Architecture)는 이러한 선택에 현실적인 한계를 설정합니다. 카메라, 고밀도 포인트 클라우드, 레이더 텐서(Radar Tensor), 고주파 고유수용감각 스트림은 서로 매우 다른 메모리 및 처리 워크로드를 생성합니다. 선택된 엣지 컴퓨터(Edge Computer)는 종단간 지연시간 예산(End-to-End Latency Budget) 내에서 이러한 데이터 스트림을 수집하고, 전처리하고, 동기화하고, 융합하고, 추론해야 합니다. 이론적으로 우수한 센서 구성이라도 적시에 물리적 행동을 지원할 만큼 빠르게 데이터를 처리할 수 없다면 시스템 수준에서는 오히려 열등한 아키텍처가 됩니다.

전력(Power)과 열 제약(Thermal Constraint)은 센서를 전체 피지컬 AI 플랫폼과 더욱 긴밀하게 연결합니다. 센서는 직접 전력을 소비하지만 더 큰 영향은 센서 데이터를 처리하기 위해 추가되는 컴퓨팅 부하에서 발생할 수 있습니다. 따라서 더 높은 센싱 능력이 배터리 운용시간(Battery Runtime)을 감소시키거나 냉각 요구사항(Cooling Requirement)을 증가시킬 수 있습니다. 모달리티 선택에서는 센서 자체의 소비전력만 독립적으로 평가하지 않고 센서, 통신, 메모리, 추론을 포함한 전체 에너지 소비를 고려해야 합니다.

실용적인 선택 과정은 반복적(Iterative)이어야 합니다. 설계자는 먼저 임무에 필요한 세계 상태와 로봇 상태를 식별하고, 어떤 상태를 직접 관측할 수 있는지 판단하며, 추가적인 증거가 필요한 불확실성이나 고장 조건을 식별합니다. 이후 후보 모달리티를 정보 가치, 공간 및 시간 커버리지, 강건성(Robustness), 중복성, 대역폭, 컴퓨팅, 전력, 비용, 통합 복잡도(Integration Complexity)를 기준으로 평가합니다. AI 성능을 검증한 후 요구사항이 충족되지 않으면 센싱 구성을 다시 수정합니다.

결과적으로 하나의 아키텍처가 서로 다른 운용 수준(Operational Level)에 따라 의도적으로 서로 다른 센싱 구성을 사용할 수도 있습니다. 주 센서(Primary Sensor)는 정상적인 고성능 운용을 지원하고, 보완 센서(Complementary Sensor)는 불확실성을 감소시키며, 독립적인 측정 수단은 성능 저하 또는 폴백 동작(Fallback Operation)을 지원할 수 있습니다. 이를 통해 AI 시스템은 특정 센서를 사용할 수 없게 되었을 때 개별 센서 고장을 즉각적인 자율 기능 상실로 처리하지 않고 인지 전략(Perception Strategy)을 적응적으로 변경할 수 있습니다.

따라서 센서 모달리티 선택은 정보(Information), 물리(Physics), AI, 컴퓨팅(Compute), 시스템 제약(System Constraint)을 동시에 고려하는 최적화 문제입니다. 가장 좋은 구성은 반드시 가장 많은 모달리티나 가장 높은 사양을 가진 구성이 아닙니다. 임무에 중요한 물리 상태를 충분히 관측 가능하게 만들고, 불확실성과 고장에 대한 적절한 강건성을 확보하면서도 허용 가능한 지연시간, 대역폭, 전력, 열, 비용, 컴퓨팅 예산 안에서 AI에 필요한 정보를 제공하는 구성이 최적의 구성입니다.

센서-AI 공동설계(Sensor-AI Co-Design)에서 모달리티 선택은 이후의 카메라(Camera), 라이다(LiDAR), 레이더(Radar), IMU 및 고유수용감각 센싱(Proprioceptive Sensing), 센서 배치와 시야각, 공간 커버리지와 중복성, 해상도와 갱신율, 대역폭, 동기화, 센서 융합(Sensor Fusion), 성능 저하 운용(Degraded Operation), 정량적 자원 예산화(Quantitative Resource Budgeting)를 설계하기 위한 기반을 형성합니다. 핵심 원칙은 명확합니다. 센서는 하드웨어가 얼마나 많은 원시 데이터(Raw Data)를 생성할 수 있는지가 아니라, 지능(Intelligence)이 어떤 정보를 필요로 하는가를 기준으로 선택해야 합니다.

## 04.03. Camera LiDAR Radar IMU and Proprioception

![](images/image5.png){width="7.268055555555556in" height="7.268055555555556in"}

카메라(Camera)는 외형(Appearance), 질감(Texture), 색상(Color), 형태(Shape), 조명(Illumination), 공간적 맥락(Spatial Context)에 대한 밀도 높은 정보를 획득하기 때문에 많은 피지컬 AI(Physical AI) 시스템에서 핵심적인 의미 기반 센싱 모달리티(Semantic Sensing Modality)로 사용됩니다. 현대의 신경망 기반 인지 모델(Neural Perception Model)은 이미지 스트림(Image Stream)을 객체 탐지(Object Detection), 의미론적 분할(Semantic Segmentation), 깊이 추정(Depth Estimation), 운동 단서(Motion Cue), 장면 표현(Scene Representation)으로 변환할 수 있습니다. 정보 밀도는 매우 높지만 성능은 여전히 가시성과 환경 조건에 크게 영향을 받습니다.

카메라 아키텍처(Camera Architecture)는 단안(Monocular), 스테레오(Stereo), RGB-D, 적외선(Infrared), 이벤트 기반(Event-Based), 다중 카메라(Multi-Camera) 구성 등을 포함할 수 있습니다. 단안 카메라는 저렴하면서 풍부한 정보를 제공하지만 미터 단위의 깊이(Metric Depth)를 직접 측정하지는 않습니다. 스테레오 시스템은 기하학적 대응(Geometric Correspondence)을 통해 깊이를 추정하며, RGB-D 장치는 시각 정보와 명시적인 깊이 측정을 결합합니다. 적절한 구성은 운용 거리, 요구 정확도, 환경 조건, 대역폭(Bandwidth), 사용 가능한 컴퓨팅 자원(Compute Resource)에 따라 달라집니다.

카메라 성능은 조명 변화, 그림자, 눈부심(Glare), 어둠, 모션 블러(Motion Blur), 비, 안개, 오염, 가림(Occlusion)의 영향을 받습니다. 해상도(Resolution)나 프레임 레이트(Frame Rate)를 높이면 공간적 또는 시간적 정보를 개선할 수 있지만 동시에 통신 대역폭, 메모리 트래픽(Memory Traffic), 전처리(Preprocessing), 신경망 추론(Neural Inference) 워크로드가 증가합니다. 따라서 카메라 선택은 메가픽셀, 프레임 레이트, 시야각(Field of View)을 독립적인 센싱 품질 지표로 다루기보다 전체 인지 파이프라인(Perception Pipeline)을 고려해야 합니다.

라이다(LiDAR)는 센서와 주변 표면 사이의 거리를 추정하여 직접적인 기하학적 측정(Geometric Measurement)을 제공합니다. 여러 스캐닝 방향에서 반복적으로 측정하면 환경의 3차원 구조를 표현하는 포인트 클라우드(Point Cloud)가 생성됩니다. 이러한 특성으로 라이다는 정확한 공간 관계가 시각적 외형보다 중요한 위치 추정(Localization), 매핑(Mapping), 장애물 탐지(Obstacle Detection), 자유 공간 추정(Free-Space Estimation), 지형 분석(Terrain Analysis), 기하학적 월드 모델링(Geometric World Modeling)에 특히 유용합니다.

카메라와 달리 라이다는 기하 구조를 복원하기 위해 자연적인 장면 조명(Scene Illumination)에 주로 의존하지 않습니다. 그러나 라이다 측정에도 제한된 각도 해상도(Angular Resolution), 거리가 증가할수록 발생하는 희소 샘플링(Sparse Sampling), 가림, 표면의 반사 특성, 환경 간섭(Environmental Interference), 측정 불확실성(Measurement Uncertainty) 등의 한계가 존재합니다. 또한 생성되는 포인트 클라우드는 이미지와 구조적으로 다르기 때문에 특화된 전처리, 공간 인덱싱(Spatial Indexing), 복셀화(Voxelization), 투영(Projection), 포인트 기반 신경망(Point-Based Neural Architecture)이 필요할 수 있습니다.

라이다 구성은 로봇의 공간적 규모와 동역학(Dynamics)에 맞추어야 합니다. 탐지 거리(Detection Range), 수직 및 수평 시야각, 각도 해상도, 스캔 주파수(Scan Frequency), 포인트 밀도(Point Density), 측정 정확도가 어떤 기하학적 상태를 관측할 수 있는지를 결정합니다. 고속 실외 플랫폼은 충분한 반응 시간을 확보하기 위해 장거리 탐지가 필요할 수 있는 반면, 실내 자율이동로봇(AMR)은 근거리 커버리지(Near-Field Coverage), 소형 패키징, 장애물 및 주행 통로 주변의 정확한 기하 정보가 더 중요할 수 있습니다.

레이더(Radar)는 무선주파수 에너지(Radio-Frequency Energy)를 송신하고 반사된 신호를 측정하여 광학 및 레이저 센싱을 보완합니다. 레이더 아키텍처에 따라 거리(Range), 방위각(Azimuth), 고도각(Elevation), 방사 방향 속도(Radial Velocity)를 측정할 수 있습니다. 특히 도플러 측정(Doppler Measurement)을 통한 상대 속도에 대한 직접적인 감지 능력은 접근하거나 멀어지거나 횡단하는 객체를 식별해야 하는 동적 환경에서 예측(Prediction), 충돌 회피(Collision Avoidance), 운동 인식 월드 모델링(Motion-Aware World Modeling)에 중요한 가치를 제공합니다.

레이더는 카메라의 성능이 크게 저하되는 어둠이나 다양한 저가시성 환경에서도 유용한 센싱 능력을 유지할 수 있습니다. 그러나 레이더 관측은 공간 해상도, 잡음 특성(Noise Characteristics), 반사(Reflection), 다중경로 효과(Multipath Effect), 의미 정보의 풍부함 측면에서 일반적인 이미지나 라이다 포인트 클라우드와 크게 다릅니다. 따라서 AI 모델은 레이더를 단순히 다른 기하 센서의 저해상도 대체재로 취급하기보다 레이더의 관측 증거가 객체 및 움직임과 어떻게 대응되는지를 학습해야 합니다.

관성측정장치(Inertial Measurement Unit, IMU)는 주변 객체를 직접 설명하기보다 센싱 플랫폼 자체의 움직임을 관측합니다. 가속도계(Accelerometer)는 비력(Specific Acceleration)을 측정하고 자이로스코프(Gyroscope)는 각속도를 측정하며, 일반적으로 카메라나 스캐닝 라이다보다 훨씬 높은 주파수로 동작합니다. 이러한 측정은 자세(Orientation), 속도 변화(Velocity Change), 자기 운동(Ego-Motion), 안정화(Stabilization), 서로 다른 시간에 획득된 환경 관측값의 좌표 변환을 추정하는 데 필수적인 단기 운동 정보를 제공합니다.

IMU 측정은 높은 주파수를 제공하지만 불완전하기 때문에 중요한 가치를 가집니다. 바이어스(Bias), 스케일 팩터 오차(Scale-Factor Error), 진동, 온도 영향, 적분 드리프트(Integration Drift), 잡음으로 인해 관성 측정만을 사용할 경우 시간이 지날수록 오차가 누적됩니다. 따라서 피지컬 AI 시스템은 일반적으로 관성 정보를 카메라, 라이다, 위성항법시스템(GNSS), 엔코더(Encoder) 등의 관측과 결합합니다. 센서 융합(Sensor Fusion)을 사용하면 고주파 관성 전파(Inertial Propagation)를 수행하면서 더 강한 절대적 또는 기하학적 제약을 제공하는 측정값으로 주기적으로 오차를 보정할 수 있습니다.

고유수용감각(Proprioception)은 외부 환경의 인지를 로봇의 물리적 신체 내부에 대한 센싱으로 확장합니다. 휠 및 관절 엔코더, 모터 위치와 속도, 전류, 토크, 힘, 촉각 측정(Tactile Measurement), 액추에이터 온도(Actuator Temperature), 배터리 상태(Battery State) 등의 내부 신호는 체화된 시스템(Embodiment)이 실제로 어떻게 동작하고 있는지를 설명합니다. 이러한 측정값을 통해 AI 시스템은 명령된 행동(Commanded Action)과 실제 실행된 행동(Executed Action)을 구분하고 외부 센서만으로는 신뢰성 있게 판단하기 어려운 물리적 상태를 추론할 수 있습니다.

휠 기반 로봇(Wheeled Robot)에서는 엔코더 측정으로 바퀴 회전과 오도메트리 운동(Odometric Motion)을 추정할 수 있지만 슬립(Slip)과 불규칙한 지형으로 인해 휠 움직임과 실제 차량 이동 사이에 차이가 발생할 수 있습니다. 매니퓰레이터(Manipulator)와 휴머노이드(Humanoid)에서는 관절 엔코더가 운동학적 구성(Kinematic Configuration)을 설명하고 힘 및 토크 측정이 접촉과 하중을 나타냅니다. 4족 보행 로봇(Quadruped)에서는 관절 상태, IMU, 모터 전류, 접촉 정보가 함께 신체 상태 추정(Body-State Estimation), 균형, 보행(Locomotion), 지형 상호작용에 기여합니다.

이러한 모달리티 사이의 근본적인 차이는 각각 어떤 물리적 속성을 관측 가능하게 만드는가에 있습니다. 카메라는 외형과 의미 정보에 특히 강하고, 라이다는 명시적인 기하 정보, 레이더는 거리와 상대 운동, IMU는 빠른 관성 동역학(Inertial Dynamics), 고유수용감각은 내부 체화 상태(Internal Embodiment State)와 물리적 상호작용에 강점을 가집니다. 어떤 하나의 모달리티도 로봇과 환경을 완전하게 설명하지 못하며, 서로 다른 물리 변수에 대한 상호 보완적인 관측 가능성(Complementary Observability)을 통해 가치가 형성됩니다.

다중모달 융합(Multimodal Fusion)은 이러한 상호 보완적인 측정값을 결합하여 더욱 일관된 상태 표현(State Representation)을 생성합니다. 카메라 특징(Camera Feature)은 기하학적 측정값에 의미 정보를 연결하고, 라이다는 공간 구조를 제약하며, 레이더는 운동 정보를 강화할 수 있습니다. IMU 데이터는 플랫폼의 움직임을 보상하고, 고유수용감각은 로봇의 실제 물리적 구성에 상태 추정을 고정합니다. 융합은 시스템 요구사항에 따라 원시 데이터(Raw Data), 특징(Feature), 잠재 표현(Latent Representation), 객체(Object), 상태 추정, 의사결정(Decision) 수준에서 수행될 수 있습니다.

센서 융합이 정확한 캘리브레이션(Calibration)과 동기화(Synchronization)의 필요성을 제거하는 것은 아닙니다. 카메라 이미지, 라이다 포인트, 레이더 탐지, IMU 측정, 고유수용감각 신호는 서로 다른 좌표계(Coordinate Frame)와 서로 다른 시간에 생성됩니다. 내·외부 캘리브레이션(Intrinsic and Extrinsic Calibration)은 이들 사이의 공간 관계를 설정하고, 타임스탬핑(Timestamping)과 동기화는 시간 관계를 설정합니다. 이러한 기반이 정확하지 않으면 정교한 AI 융합 모델이라도 물리적으로 일관되지 않은 측정값을 결합하여 신뢰하기 어려운 세계 상태 추정을 생성할 수 있습니다.

센서 데이터 전송률(Data Rate) 역시 크게 다릅니다. 고해상도 다중 카메라 시스템은 대용량의 연속 이미지 스트림을 생성하고, 라이다는 구조화된 기하학적 측정값을 생성하며, 레이더는 탐지 데이터 또는 텐서 표현(Tensor Representation)을 생성합니다. IMU와 엔코더는 데이터 크기는 상대적으로 작지만 훨씬 높은 주파수의 스트림을 생성합니다. 따라서 센싱 아키텍처는 요구되는 종단간 지연시간(End-to-End Latency) 안에서 유용한 정보를 유지할 수 있도록 충분한 통신 대역폭, 버퍼링, 메모리 대역폭(Memory Bandwidth), 전처리 능력, 컴퓨팅 스케줄링(Compute Scheduling)을 제공해야 합니다.

중복성(Redundancy)은 이러한 모달리티가 사용하는 서로 다른 물리적 원리를 활용해야 합니다. 시각 정보가 신뢰하기 어려워졌을 때 기하 정보, 레이더 또는 관성 정보가 상태 추정을 계속 제약할 수 있습니다. 슬립으로 휠 오도메트리(Wheel Odometry)가 부정확해지면 IMU와 외부 환경 센싱이 이러한 불일치를 감지할 수 있습니다. 외부 인지가 일시적으로 가려지더라도 고유수용 상태와 시간 기반 월드 모델(Temporal World Model)이 짧은 시간 동안 상태의 연속성을 유지할 수 있습니다. 강건성(Robustness)은 단순한 센서 개수가 아니라 상호 보완적인 증거에서 형성됩니다.

AI는 이러한 모달리티를 활용하는 방법을 더욱 변화시킬 수 있습니다. 학습 기반 인지 모델(Learned Perception Model)은 카메라에서 의미적·기하학적 특징을 추출하고, 신경망 기반 포인트 클라우드 모델(Neural Point-Cloud Model)은 라이다를 해석하며, 레이더 네트워크(Radar Network)는 의미 있는 운동 패턴을 식별할 수 있습니다. 다중모달 트랜스포머(Multimodal Transformer)는 이질적인 데이터 스트림에서 공유 잠재 표현(Shared Latent Representation)을 구축할 수 있으며, 시간 모델과 월드 모델은 이러한 표현을 시간에 걸쳐 통합하여 개별 측정에서 부분적으로만 관측 가능한 상태를 추정할 수 있습니다.

적절한 센서 조합은 궁극적으로 임무(Task)와 체화(Embodiment)에 따라 결정됩니다. 실내 자율이동로봇은 카메라, 라이다, IMU, 휠 엔코더에 크게 의존할 수 있으며, 실외 자율 플랫폼(Outdoor Autonomous Platform)은 추가적인 레이더와 더 넓은 공간 커버리지의 이점을 얻을 수 있습니다. 모바일 매니퓰레이터(Mobile Manipulator)는 관절, 힘, 접촉 센싱을 추가하며, 4족 보행 로봇과 휴머노이드는 신체, 지형, 객체, 주변 환경 사이의 동적 상호작용을 지원하기 위해 더욱 풍부한 고유수용감각을 필요로 합니다.

따라서 카메라, 라이다, 레이더, IMU, 고유수용감각은 서로 독립된 센싱 제품이 아니라 하나의 정보 아키텍처(Information Architecture)를 구성하는 요소로 이해해야 합니다. 각 센서의 해상도, 거리, 갱신율(Update Rate), 배치(Placement), 동기화, 대역폭, 컴퓨팅 요구량, 불확실성, 고장 특성(Failure Characteristics)은 인지 및 월드 모델 알고리즘과 공동설계(Co-Design)되어야 합니다. 목표는 물리 플랫폼의 자원 제약을 만족하면서 임무에 중요한 상태를 충분한 정확도와 적시성(Timeliness)으로 관측 가능하게 만드는 것입니다.

이러한 다중모달 관점(Multimodal Perspective)은 이후 센서-AI 공동설계(Sensor-AI Co-Design)에서 다루게 될 센서 배치와 시야각, 공간 커버리지와 중복성, 해상도와 갱신율, 센서 대역폭과 컴퓨팅 부하, 시간 동기화(Time Synchronization), 센서 융합, 모델 주도 센서 선택(Model-Driven Sensor Selection), 성능 저하 운용(Degraded Operation), 정량적 자원 예산화(Quantitative Resource Budgeting)의 기술적 기반을 형성합니다. 핵심 설계 질문은 어떤 센서가 보편적으로 가장 우수한가가 아니라, 어떤 센서 조합이 신뢰성 있는 피지컬 AI에 필요한 정보를 제공하는가입니다.

## 04.04. Sensor Placement and Field of View

![](images/image6.png){width="7.268055555555556in" height="7.268055555555556in"}

센서 배치(Sensor Placement)는 피지컬 AI(Physical AI) 아키텍처의 핵심 요소입니다. 센싱 성능은 센서가 무엇을 측정할 수 있는가뿐만 아니라 로봇의 어디에, 어떤 방식으로 장착되는가에도 크게 좌우되기 때문입니다. 고성능 카메라(Camera), 라이다(LiDAR), 레이더(Radar) 또는 다른 센서라도 시야가 가려지거나 방향이 부적절하고, 과도한 진동에 노출되거나 인지(Perception), 계획(Planning), 제어(Control)에 중요한 영역을 바라보지 못한다면 유용한 정보를 충분히 제공할 수 없습니다.

시야각(Field of View)은 센서가 유용한 관측값을 획득할 수 있는 공간적 영역을 정의합니다. 일반적으로 수평 및 수직 각도 범위(Horizontal and Vertical Angular Coverage)로 표현하지만 실제 유효 커버리지(Effective Coverage)는 센싱 거리(Sensing Range), 장착 높이(Mounting Height), 방향(Orientation), 로봇 형상(Robot Geometry), 환경 구조(Environmental Structure)의 영향도 받습니다. 따라서 중요한 설계 질문은 단순히 명목상 시야각이 얼마나 넓은가가 아니라 실제 로봇 운용 중 임무에 중요한 영역이 얼마나 지속적으로 관측 가능한가입니다.

센서 배치는 임무 중심의 관측 가능성 요구사항(Task-Oriented Observability Requirements)에서 시작해야 합니다. 내비게이션(Navigation)에는 자유 공간(Free Space), 장애물, 지형, 잠재적인 충돌 영역을 충분히 관측할 수 있어야 하며, 조작(Manipulation)에는 작업 공간(Workspace), 엔드 이펙터(End Effector), 대상 객체, 접촉 표면에 대한 상세한 관측이 필요합니다. 고속 이동에서는 더 먼 전방 시야가 중요하고, 전방향 상호작용(Omnidirectional Interaction)에서는 넓은 주변 커버리지가 필요할 수 있습니다. 따라서 센서의 기하학적 구성은 AI가 추정해야 하는 상태를 기준으로 결정되어야 합니다.

로봇의 체화(Embodiment)는 센서 배치에 물리적 제약을 만듭니다. 섀시(Chassis), 바퀴, 매니퓰레이터(Manipulator), 페이로드(Payload), 보호 구조물, 안테나, 움직이는 관절 등이 센서 시야를 가리거나 변화하는 사각 영역(Blind Region)을 만들 수 있습니다. 독립된 기계 모델에서는 최적으로 보이는 센서 위치도 로봇이 화물을 운반하거나 로봇 팔을 움직이면 효과적이지 않을 수 있습니다. 따라서 센서-AI 공동설계(Sensor-AI Co-Design)에서는 하나의 정적인 자세가 아니라 실제 로봇의 다양한 구성 상태에서 시야각을 평가해야 합니다.

장착 높이(Mounting Height)는 중요한 기하학적 절충 관계(Geometric Tradeoff)를 형성합니다. 높은 위치에 장착된 카메라나 라이다는 가까운 장애물 너머를 더 멀리 관측하여 전체적인 장면 커버리지(Scene Coverage)를 향상시킬 수 있지만 진동, 기계적 충격, 구조 변형에 더 취약해질 수 있습니다. 낮은 위치의 센서는 연석(Curb), 바닥 수준의 장애물, 발 디딤 위치(Foothold), 근거리 기하 구조를 더 잘 관측할 수 있지만 가림에 더 취약할 수 있습니다. 따라서 하나의 높이에서 모든 인지 요구사항을 만족할 수 없다면 서로 다른 높이의 복수 관측점을 활용할 수 있습니다.

센서 방향(Orientation)은 사용 가능한 각도 해상도(Angular Resolution)가 환경에 어떻게 분배되는지를 결정합니다. 전방 센서는 예상 이동 방향에 정보를 집중시키는 반면, 측면 및 후방 센서는 회전, 후진, 추월, 도킹(Docking), 상호작용 과정에서 주변 인식을 향상시킵니다. 하향 카메라는 지형이나 발 디딤 위치 추정에 활용될 수 있으며, 매니퓰레이터나 휴머노이드(Humanoid)에서는 상향 또는 비스듬한 시야도 중요할 수 있습니다. 센서 방향은 예상되는 이동과 상호작용의 기하 구조를 반영해야 합니다.

근거리 커버리지(Near-Field Coverage)는 특별히 주의해야 합니다. 많은 센서는 최소 유효 거리(Minimum Useful Range)나 센서 기하 구조 때문에 로봇과 매우 가까운 영역을 충분히 관측하지 못할 수 있습니다. 이러한 영역은 저속 기동, 도킹, 조작, 인간-로봇 상호작용(Human-Robot Interaction), 충돌 방지에 특히 중요합니다. 따라서 장거리 인지 시스템이 효과적으로 관측하지 못하는 영역을 보완하기 위해 광각 카메라(Wide-Angle Camera), 단거리 깊이 센싱(Short-Range Depth Sensing), 초음파 센싱(Ultrasonic Sensing), 촉각 정보(Tactile Information), 중첩된 센서 시야가 필요할 수 있습니다.

사각 영역(Blind Zone)은 물리적 구조물이나 제한된 센서 각도로 인해 중요한 영역을 관측하지 못할 때 발생합니다. 섀시 뒤쪽에 가려진 영역처럼 정적인 사각 영역도 있지만 매니퓰레이터, 조향 장치, 페이로드 또는 사람이 센서 시야를 통과하면서 발생하는 동적 사각 영역(Dynamic Blind Zone)도 존재합니다. AI 시스템은 로봇 주변의 인지 품질이 균일하다고 가정해서는 안 됩니다. 센싱 커버리지를 평가할 때 사각 영역의 기하 구조와 불확실성(Uncertainty)을 명시적으로 표현해야 합니다.

중첩 시야(Overlapping Field of View)는 연속성과 중복성(Redundancy)을 동시에 제공할 수 있습니다. 인접한 카메라 사이의 이미지 영역이 중첩되면 카메라 간 연관(Cross-Camera Association)과 더욱 연속적인 객체 추적(Object Tracking)을 지원할 수 있으며, 라이다 또는 깊이 센서의 중첩 커버리지는 기하학적 공백을 줄일 수 있습니다. 그러나 지나친 중첩은 동일한 영역을 반복적으로 관측하여 센싱, 대역폭, 컴퓨팅 자원을 낭비할 수 있습니다. 따라서 필요한 중첩 수준은 캘리브레이션(Calibration), 추적 연속성, 중복성 요구사항, 고장 허용성(Fault Tolerance)에 따라 결정해야 합니다.

센서 배치는 다중모달 융합(Multimodal Fusion)에도 영향을 줍니다. 카메라, 라이다, 레이더의 관측 영역이 충분히 중첩되면 AI는 동일한 물리 객체에 대한 의미 정보(Semantic Information), 기하 정보(Geometric Information), 운동 정보(Motion Information)를 서로 연관시킬 수 있습니다. 거의 완전히 분리된 시야를 가진 센서도 개별적으로는 유용할 수 있지만 직접적인 교차 모달 증거(Cross-Modal Evidence)를 제공하는 능력은 감소합니다. 따라서 다중모달 센서 배치에서는 개별 센서의 커버리지뿐만 아니라 공통 관측 가능 공간(Common Observable Volume)도 고려해야 합니다.

공간 캘리브레이션(Spatial Calibration)은 센서 배치와 분리할 수 없습니다. 각 센서는 고유한 좌표계(Coordinate Frame)를 가지며 로봇에 대한 센서의 위치와 방향을 정확히 알아야 합니다. 외부 캘리브레이션(Extrinsic Calibration)은 카메라, 라이다, 레이더, IMU, 로봇 기준 좌표계(Robot-Base Frame) 사이의 변환 관계를 설정합니다. 작은 장착 오차도 장거리에서는 상당한 공간적 불일치로 확대되어 서로 다른 모달리티의 객체 경계, 포인트 클라우드(Point Cloud), 운동 추정값이 잘못 정렬될 수 있습니다.

기계적 안정성(Mechanical Stability)은 캘리브레이션 품질에 직접적인 영향을 줍니다. 조립 단계에서 센서를 정확하게 캘리브레이션하더라도 진동, 충격, 열팽창(Thermal Expansion), 구조적 휨(Structural Flex), 유지보수, 반복적인 기계 하중으로 인해 시간이 지나면서 센서 자세가 변할 수 있습니다. 따라서 장착 구조는 요구되는 인지 정확도를 유지할 수 있을 정도의 강성을 확보해야 합니다. 정밀한 기하 융합(Geometric Fusion)이 필요한 시스템에서는 배치 이후 변화를 탐지하기 위한 캘리브레이션 모니터링(Calibration Monitoring)이나 온라인 캘리브레이션(Online Calibration)이 필요할 수도 있습니다.

진동(Vibration)은 캘리브레이션 드리프트(Calibration Drift) 이외의 문제도 발생시킵니다. 카메라 진동은 모션 블러나 롤링 셔터 왜곡(Rolling-Shutter Distortion)을 유발할 수 있으며, 라이다나 레이더 장착부가 흔들리면 측정값과 가정된 센서 좌표계 사이의 관계가 달라질 수 있습니다. IMU는 실제 차량 운동과 무관한 구조 진동까지 측정할 수 있습니다. 따라서 기계적 절연(Mechanical Isolation), 강성 장착(Rigid Mounting), 필터링, 노출 제어(Exposure Control), 운동 보상(Motion Compensation)을 소프트웨어 또는 하드웨어의 개별 문제로 분리하지 않고 함께 고려해야 합니다.

환경 노출(Environmental Exposure) 역시 센서 배치를 제한합니다. 카메라와 라이다는 비, 먼지, 진흙, 결로(Condensation), 직사광선, 오염으로부터 보호가 필요할 수 있으며, 레이더 배치에서는 무선주파수 전송에 영향을 주는 재료를 고려해야 합니다. 보호창(Protective Window)과 커버 자체도 반사, 감쇠(Attenuation), 왜곡, 오염을 발생시킬 수 있습니다. 따라서 기하학적으로 이상적인 위치라도 예상 운용 환경에서 신뢰성 있는 센싱을 유지할 수 없다면 적절한 배치 위치라고 할 수 없습니다.

센서 배치 결정은 유지보수성과 수명주기 신뢰성(Lifecycle Reliability)에도 영향을 줍니다. 센서는 과도한 분해 작업 없이 청소, 검사, 교체, 캘리브레이션, 커넥터 정비(Connector Servicing)가 가능해야 합니다. 동시에 지나치게 노출된 위치는 센서 손상 위험을 증가시킬 수 있습니다. 지속적인 운용을 목표로 하는 피지컬 AI 플랫폼은 인지 품질과 정비성(Serviceability), 강건성(Robustness)을 균형 있게 고려해야 합니다. 센싱 능력을 지속적으로 유지할 수 없다면 결국 AI 신뢰성 문제로 이어지기 때문입니다.

동적 로봇(Dynamic Robot)에서는 움직임 전체에 걸쳐 시야각을 분석해야 합니다. 회전, 가속, 차체 롤(Body Roll), 서스펜션 움직임, 보행(Locomotion), 매니퓰레이터 동작 과정에서 센서와 환경 사이의 방향 및 가림 관계가 크게 변할 수 있습니다. 4족 보행 로봇(Quadruped)의 신체 자세는 카메라가 지형을 바라보는 각도를 지속적으로 변화시킬 수 있으며, 휴머노이드의 머리와 몸통 움직임도 시각적 커버리지를 계속 변화시킵니다. 따라서 정적인 센서 배치도만으로 동적인 체화 시스템을 충분히 평가할 수 없습니다.

센서 배치에서는 센싱 거리(Sensing Range)와 반응 거리(Reaction Distance)의 관계도 고려해야 합니다. 고속 로봇은 위험 요소에 도달하기 전에 인지, 불확실성 감소, 계획, 제동을 수행할 수 있도록 충분한 전방 커버리지가 필요합니다. 예상 이동 방향을 향한 장거리 센싱은 위험 요소에 대한 조기 정보를 제공하고, 근거리 센서는 로봇 바로 주변을 보호합니다. 따라서 유효 커버리지는 전체 센싱-행동 지연시간(Sensing-to-Action Latency)과 로봇 동역학을 함께 고려하여 평가해야 합니다.

AI 모델은 공간 정보를 서로 다른 방식으로 활용하기 때문에 AI 모델 구조(AI Model Architecture) 역시 센서 배치 요구사항에 영향을 줄 수 있습니다. 조감도 인지(Bird\'s-Eye-View Perception), 점유 네트워크(Occupancy Network), 다중 카메라 트랜스포머(Multi-Camera Transformer), 다중모달 월드 모델(Multimodal World Model)은 센서 사이의 예측 가능한 중첩과 기하학적 관계에 의존할 수 있습니다. 독립적인 객체 탐지기(Object Detector)에 최적화된 센서 배치가 통합 공간 표현(Unified Spatial Representation)에는 최적이 아닐 수 있습니다. 따라서 하드웨어 기하 구조는 사용할 AI 표현 및 융합 아키텍처와 함께 평가해야 합니다.

센서 배치는 데이터 기반 평가(Data-Driven Evaluation)를 통해 최적화할 수도 있습니다. 후보 구성을 시뮬레이션(Simulation)이나 기록된 환경에서 시험하여 가시 영역(Visible Area), 객체 탐지 가능성(Object Detectability), 가림 빈도(Occlusion Frequency), 기하학적 불확실성, 추적 연속성(Tracking Continuity), 후속 AI 성능을 측정할 수 있습니다. 이후 센서 위치를 하드웨어 비용, 대역폭, 전력, 컴퓨팅 요구량, 기계적 제약과 비교할 수 있습니다. 이를 통해 센서 배치를 직관적인 기계 설계 문제가 아니라 측정 가능한 시스템 최적화(System Optimization) 문제로 전환할 수 있습니다.

중복성은 센서 배치에 또 다른 설계 차원을 추가합니다. 동일한 모달리티의 센서 두 개는 하드웨어 중복성을 제공할 수 있지만 거의 동일한 시점을 공유한다면 동일한 가림이나 오염으로 동시에 성능이 저하될 수 있습니다. 공간적으로 다양한 배치(Spatially Diverse Placement)는 서로 다른 관측선(Line of Sight)을 확보하며, 다중모달 다양성(Multimodal Diversity)은 서로 다른 물리적 센싱 원리를 추가합니다. 따라서 강건한 아키텍처는 고장 허용 커버리지를 설계할 때 센서 다양성과 관측점 다양성(Viewpoint Diversity)을 함께 고려해야 합니다.

최적의 구성은 일반적으로 아무런 제약 없이 시야각을 최대화하는 구성이 아닙니다. 더 넓은 커버리지를 확보하려면 추가 센서, 고해상도 처리, 더 큰 대역폭, 더 많은 캘리브레이션 관계, 증가된 컴퓨팅 자원이 필요할 수 있습니다. 목표는 비용, 전력, 열(Thermal), 통신, 기계 구조, 컴퓨팅 예산 안에서 임무에 중요한 공간을 충분히 관측하고, 사각 영역을 통제하며, 유용한 중첩과 적절한 중복성을 확보하고, 불확실성을 허용 가능한 수준으로 유지하는 것입니다.

따라서 센서 배치와 시야각은 센서 모달리티 선택(Sensor Modality Selection)과 이후의 공간 커버리지 및 중복성(Spatial Coverage and Redundancy), 센서 해상도 및 갱신율(Sensor Resolution and Update Rate), 대역폭 및 컴퓨팅 부하(Bandwidth and Compute Load), 동기화(Synchronization), 융합 아키텍처(Fusion Architecture), 성능 저하 운용(Degraded Operation), 정량적 자원 예산화(Quantitative Resource Budgeting)를 연결하는 역할을 합니다. 피지컬 AI에서는 센서가 무엇을 측정하는가만큼 어디를 바라보는가도 중요하며, 공간적 관측 가능성(Spatial Observability)이 지능이 무엇을 신뢰성 있게 알고 행동할 수 있는지를 결정합니다.

## 04.05. Spatial Coverage and Redundancy

![](images/image7.png){width="7.268055555555556in" height="7.268055555555556in"}

공간 커버리지(Spatial Coverage)는 피지컬 AI(Physical AI) 시스템이 내비게이션(Navigation), 상호작용(Interaction), 조작(Manipulation), 안전(Safety)에 중요한 영역을 얼마나 완전하게 관측할 수 있는지를 정의합니다. 이는 단순히 센서들의 명목상 시야각(Field of View)을 합한 것과 동일하지 않습니다. 실제 유효 커버리지(Effective Coverage)는 센서 배치, 센싱 거리, 가림(Occlusion), 로봇 형상, 환경 구조, 측정 품질, 움직임에 의해 결정됩니다. AI가 필요한 의사결정을 지원할 수 있을 정도의 충분한 품질로 정보를 획득할 때만 해당 영역이 의미 있게 커버되었다고 할 수 있습니다.

따라서 커버리지 요구사항(Coverage Requirement)은 임무 중요 공간(Task-Critical Space)을 기준으로 도출해야 합니다. 이동 로봇(Mobile Robot)은 전방 주행 경로, 즉각적인 충돌 영역(Collision Envelope), 측면 회전 영역, 후진 시 후방 공간을 지속적으로 관측해야 할 수 있습니다. 매니퓰레이터(Manipulator)는 작업 공간(Workspace), 대상 객체, 접촉 영역에 대한 상세한 센싱이 필요합니다. 4족 보행 로봇(Quadruped)은 추가적으로 지형과 발 디딤 위치(Foothold) 정보가 필요하며, 휴머노이드(Humanoid)는 전신 움직임과 상호작용에 따라 변화하는 커버리지가 필요합니다.

공간 커버리지는 여러 거리 범위에서 고려할 수 있습니다. 근거리 센싱(Near-Field Sensing)은 로봇 바로 주변을 보호하며 도킹(Docking), 정밀 기동, 조작, 인간과의 상호작용을 지원합니다. 중거리 센싱(Mid-Range Sensing)은 로컬 내비게이션(Local Navigation)과 장애물 회피(Obstacle Avoidance)에 필요한 정보를 제공하며, 장거리 센싱(Long-Range Sensing)은 예측과 계획을 위한 추가 시간을 제공합니다. 객체가 하나의 센싱 거리에서 다른 거리로 이동할 때 사라지지 않도록 이러한 계층 사이에는 충분한 중첩(Overlap)이 필요합니다.

커버리지 설계에서는 방향성(Directionality)도 고려해야 합니다. 전진 이동에서는 일반적으로 이동 방향에 더 긴 센싱 거리와 높은 해상도를 배치하는 것이 합리적이지만 피지컬 AI 플랫폼이 항상 전진만 하는 것은 아닙니다. 회전, 후진, 측면 이동, 조작, 상호작용은 추가적인 관측 공간 요구사항을 만듭니다. 일부 로봇에서는 전방향 커버리지(Omnidirectional Coverage)가 유용할 수 있지만 완전히 균일한 커버리지는 불필요하게 비용을 증가시킬 수 있습니다. 따라서 공간 센싱은 각 영역에서 사건이 발생할 확률과 그 결과의 중요성을 반영해야 합니다.

사각 영역(Blind Zone)은 임무와 관련된 상태를 충분한 신뢰성으로 관측할 수 없는 공간을 의미합니다. 제한된 시야각, 최소 센싱 거리(Minimum Sensing Range), 섀시 형상, 바퀴, 페이로드(Payload), 매니퓰레이터, 보호 구조물, 환경적 가림 등에 의해 발생할 수 있습니다. 사각 영역 자체가 반드시 허용 불가능한 것은 아니지만 명확하게 식별되어야 합니다. 계획 및 제어 시스템은 관측되지 않은 공간을 비어 있거나 안전한 공간으로 가정하지 않고 어디에서 인지 신뢰도(Perception Confidence)가 감소하는지를 알고 있어야 합니다.

동적 사각 영역(Dynamic Blind Zone)은 체화 시스템(Embodied System)에서 특히 중요합니다. 매니퓰레이터가 일시적으로 카메라를 가릴 수 있고, 화물이 후방 센서를 차단할 수 있으며, 서스펜션 움직임이 센서 방향을 변화시킬 수 있습니다. 보행 로봇의 움직임은 센서와 지형 사이의 관계를 지속적으로 변화시키며, 인간과의 상호작용에서도 예측하기 어려운 가림이 발생할 수 있습니다. 따라서 공간 커버리지는 정적인 CAD 모델(CAD Model)만을 기준으로 평가하지 않고 예상되는 로봇 구성과 움직임 전체에서 평가해야 합니다.

중복성(Redundancy)은 센서, 관측점(Viewpoint), 모달리티(Modality)를 사용할 수 없거나 신뢰할 수 없게 되었을 때 추가적인 관측 정보를 제공합니다. 그러나 중복성을 단순히 동일한 센서를 추가 설치하는 것으로 해석해서는 안 됩니다. 유용한 중복성이란 현실적인 고장이나 환경적 성능 저하가 발생하더라도 임무에 중요한 상태를 계속 관측할 수 있도록 하는 것입니다. 설계 목표는 하드웨어 자체의 복제가 아니라 정보 가용성(Information Availability)의 지속적인 유지이며, 이는 고장 허용 센서-AI 공동설계(Fault-Tolerant Sensor-AI Co-Design)의 핵심 개념입니다.

공간적 중복성(Spatial Redundancy)은 여러 관측점이 동일하거나 연관된 영역을 관측할 때 형성됩니다. 중첩된 카메라는 관측점이 전환되는 동안 객체의 가시성을 유지할 수 있고, 여러 거리 센서는 로봇 주변의 기하학적 공백을 줄일 수 있습니다. 공간적으로 분리된 센서는 서로 다른 가림 구조의 뒤쪽까지 관측할 수도 있습니다. 이러한 중복성은 연속성을 향상시키지만 과도한 중첩은 동일한 공간을 반복적으로 측정하여 하드웨어, 대역폭, 캘리브레이션(Calibration), 메모리, 추론(Inference) 비용을 증가시키면서 그에 비례하는 이점을 제공하지 못할 수 있습니다.

모달리티 중복성(Modality Redundancy)은 서로 다른 물리적 센싱 원리를 이용하여 관련된 상태를 관측합니다. 카메라는 의미 정보(Semantics)를, 라이다(LiDAR)는 기하학적 구조를, 레이더(Radar)는 거리와 속도를, 고유수용감각(Proprioception)은 로봇 신체 상태에 대한 정보를 제공할 수 있습니다. 조명 상태가 좋지 않아 시각 인지 성능이 저하되더라도 다른 모달리티가 유용한 관측 가능성(Observability)을 유지할 수 있습니다. 따라서 다양한 모달리티를 사용하는 것은 동일한 측정 원리를 공유하는 센서보다 환경 변화에 대해 더 높은 회복탄력성(Resilience)을 제공할 수 있습니다.

하드웨어 중복성(Hardware Redundancy)과 정보 중복성(Information Redundancy)도 구분해야 합니다. 동일한 카메라 두 개는 하나가 고장났을 때 하드웨어 고장 허용성(Hardware Fault Tolerance)을 제공할 수 있지만 어둠이나 공통적인 오염 조건에서는 두 카메라가 모두 무력화될 수 있습니다. 카메라와 레이더는 동일한 측정값을 생성하지 않지만 동적 객체를 탐지하는 데 필요한 정보 중복성을 함께 제공할 수 있습니다. 적절한 중복성 아키텍처는 피지컬 AI 시스템이 어떤 고장을 견뎌야 하며 어떤 상태를 계속 관측해야 하는지에 따라 결정됩니다.

공통 원인 고장(Common-Mode Failure)은 매우 중요한 고려사항입니다. 함께 장착된 센서들은 동일한 전원 공급 장치(Power Supply), 통신 링크(Communication Link), 보호창, 기계 구조물 또는 오염 환경을 공유할 수 있습니다. 따라서 여러 모달리티를 사용하더라도 의존 관계가 충분히 독립적이지 않다면 동시에 고장날 수 있습니다. 중복성 분석에서는 물리적 센서 개수뿐만 아니라 전원, 네트워크, 컴퓨팅, 타이밍(Timing), 장착 구조, 환경 노출, 소프트웨어 의존성을 함께 고려해야 합니다.

중첩(Overlap)은 다중모달 융합(Multimodal Fusion)에서도 중요한 역할을 합니다. 공통 관측 가능 공간(Shared Observable Volume)이 존재하면 카메라, 라이다, 레이더가 동일하거나 연관된 물리 객체를 측정할 수 있으며 AI가 의미적, 기하학적, 운동 정보를 서로 연관시킬 수 있습니다. 중첩이 너무 적으면 교차 모달 대응(Cross-Modal Correspondence)이 약해지고, 불필요하게 많은 중첩은 자원을 낭비합니다. 따라서 센서 배치는 전체 센싱 공간을 중복시키기보다 캘리브레이션, 연관(Association), 추적, 불확실성 감소, 융합에 필요한 수준의 공통 커버리지를 제공해야 합니다.

커버리지 품질(Coverage Quality)은 단순한 이진 상태가 아닙니다. 원거리 객체가 카메라 시야 안에 존재하더라도 신뢰성 있는 분류를 수행하기에는 픽셀 수가 부족할 수 있습니다. 라이다는 동일한 영역에 도달하더라도 매우 희소한 포인트만 제공할 수 있고, 레이더는 움직임을 감지하면서도 충분한 객체 형상 정보를 제공하지 못할 수 있습니다. 따라서 유효 공간 커버리지는 단순한 기하학적 가시성(Geometric Visibility)이 아니라 해상도, 거리에 따른 불확실성, 탐지 확률(Detection Probability), 환경 강건성(Environmental Robustness), 후속 AI 성능을 함께 고려해야 합니다.

시간적 연속성(Temporal Continuity)은 공간 커버리지의 또 다른 차원입니다. 로봇이 이동하는 동안 객체는 탐지, 연관, 추적, 예측, 계획을 수행할 수 있을 만큼 충분한 시간 동안 관측되는 것이 바람직합니다. 센서 경계가 부적절하게 구성되면 객체가 서로 다른 시야각을 통과하면서 반복적으로 사라졌다가 다시 나타날 수 있습니다. 인접 센서 사이의 적절한 중첩은 부드러운 인계(Smooth Handoff)를 제공하고 객체 정체성 상실(Identity Loss), 불안정한 상태 추정, 월드 모델 신뢰도의 급격한 변화를 감소시킬 수 있습니다.

커버리지 요구사항은 센싱-행동 지연시간(Sensing-to-Action Latency)과도 연결됩니다. 로봇 속도가 높아질수록 위험 요소에 도달하기 전에 센싱, 전처리, 추론, 융합, 예측, 계획, 제어, 물리적 반응이 완료될 수 있도록 충분히 이른 시점에 해당 영역을 관측해야 합니다. 따라서 장거리 전방 커버리지는 단순히 인지를 위한 것이 아니라 반응 시간(Reaction Time)을 확보하기 위한 것입니다. 동시에 가림으로부터 가까운 위험 요소가 갑자기 나타나거나 예상하지 못한 물체가 로봇 경로에 진입할 수 있기 때문에 근거리 중복성도 필요합니다.

안전 중요 영역(Safety-Critical Region)은 일반적인 관측 공간보다 더 강한 중복성을 요구할 수 있습니다. 바퀴, 매니퓰레이터, 접촉 표면, 인간 상호작용 영역, 예상 이동 궤적(Predicted Trajectory) 주변에는 여러 독립적인 관측이 필요할 수 있습니다. 반면 다른 영역에서는 일시적인 불확실성을 허용할 수 있습니다. 따라서 센서 자원을 위험도에 따라 집중하는 비균일 커버리지 아키텍처(Nonuniform Coverage Architecture)를 구성할 수 있으며, 피지컬 AI는 기하학적 대칭성보다 임무 중요도와 고장 결과에 따라 커버리지를 최적화해야 합니다.

AI는 커버리지와 불확실성을 명시적으로 표현할 수도 있습니다. 점유 지도(Occupancy Map), 신뢰도 필드(Confidence Field), 가시성 지도(Visibility Map), 월드 모델(World Model), 상태 추정기(State Estimator)를 이용하여 잘 관측된 영역과 불확실하거나 관측되지 않은 영역을 구분할 수 있습니다. 계획 시스템은 신뢰도가 충분하지 않을 때 속도를 낮추거나 안전 거리를 늘리고, 로봇 위치나 센서를 재배치하거나 추가 관측을 수행할 수 있습니다. 이러한 방식으로 센싱 중복성은 고정된 하드웨어 특성에 머무르지 않고 지능적 행동(Intelligent Behavior)의 능동적인 구성 요소가 됩니다.

성능 저하 운용(Degraded Operation)은 커버리지 설계 단계에서부터 고려해야 합니다. 하나의 카메라, 라이다, 레이더 또는 통신 채널이 고장나더라도 나머지 센싱 아키텍처가 감소된 수준이지만 여전히 사용 가능한 커버리지를 제공할 수 있습니다. 로봇은 속도를 낮추거나 후진을 제한하고, 특정 조작 작업을 비활성화하거나 안전 여유(Safety Margin)를 확대하거나 안전한 위치로 복귀할 수 있습니다. 시스템이 남아 있는 관측 가능성을 명확하게 정의된 성능 저하 운용 모드(Degraded Operating Mode)로 변환할 수 있을 때 중복성의 가치가 극대화됩니다.

커버리지와 중복성은 시뮬레이션(Simulation)과 실제 환경 시험을 통해 체계적으로 평가할 수 있습니다. 후보 센서 구성은 가시 체적(Visible Volume), 사각 영역 크기, 중첩 비율(Overlap Ratio), 거리에 따른 품질, 가림 빈도, 추적 연속성, 고장 커버리지(Failure Coverage), 후속 AI 성능 등을 이용하여 비교할 수 있습니다. 고장 주입(Failure Injection)을 통해 센서를 제거하거나 측정 성능을 인위적으로 저하시켜 대표적인 운용 조건에서 인지, 위치 추정, 계획, 안전 기능이 허용 범위를 유지하는지도 검증할 수 있습니다.

이러한 이점은 자원 비용(Resource Cost)과 균형을 이루어야 합니다. 추가 센서는 질량, 전력, 배선(Wiring), 네트워크 트래픽, 타임스탬핑(Timestamping) 요구사항, 캘리브레이션 관계, 전처리, 메모리 대역폭(Memory Bandwidth), 컴퓨팅 부하, 열 부하(Thermal Demand), 소프트웨어 복잡성, 유지보수, 비용을 증가시킵니다. 따라서 중복성은 전체 피지컬 AI 플랫폼을 기준으로 예산화해야 합니다. 더 많은 센싱 고장을 견딜 수 있더라도 컴퓨팅 시스템에 과부하를 주거나 운용 시간을 지나치게 감소시키는 구성이라면 전체 시스템 신뢰성을 향상시킨다고 보기 어렵습니다.

바람직한 아키텍처는 임무 중심의 충분한 커버리지, 통제된 사각 영역, 유용한 공간적 중첩, 다양한 센싱 원리, 그리고 고장 채널 사이의 적절한 독립성을 제공합니다. 예상되는 환경적 성능 저하와 선택된 하드웨어 고장이 발생하더라도 핵심적인 관측 가능성을 유지하면서 불필요한 중복은 피해야 합니다. 따라서 공간 커버리지와 중복성은 정보 가용성, 불확실성, 위험(Risk), 자원, 요구되는 운용 연속성(Operational Continuity)을 기준으로 최적화되어야 합니다.

공간 커버리지와 중복성은 센서 배치 및 시야각(Sensor Placement and Field of View)을 이후의 센서 해상도·거리·갱신율(Sensor Resolution, Range and Update Rate), 대역폭 및 컴퓨팅 부하(Bandwidth and Compute Load), 시간 동기화(Time Synchronization), 센서 융합(Sensor Fusion), 모델 주도 센서 선택(Model-Driven Sensor Selection), 성능 저하 운용, 정량적 자원 예산화(Quantitative Resource Budgeting)와 연결합니다. 핵심 목표는 모든 공간을 동일하게 관측하는 것이 아니라 센싱 조건이 이상적이지 않은 상황에서도 피지컬 AI가 반드시 알아야 하는 상태를 신뢰성 있게 관측할 수 있도록 하는 것입니다.

## 04.06. Sensor Resolution Range and Update Rate

![](images/image8.png){width="7.268055555555556in" height="7.268055555555556in"}

센서 해상도(Sensor Resolution)는 피지컬 AI(Physical AI) 시스템이 물리 세계의 구조, 외형, 움직임 또는 상태를 얼마나 세밀하게 구별할 수 있는지를 결정합니다. 높은 해상도는 더 작은 객체, 세밀한 기하학적 경계, 미묘한 표면 특성, 정밀한 상태 변화를 드러낼 수 있지만 데이터 양과 처리 요구량도 증가시킵니다. 따라서 해상도는 센서가 제공할 수 있는 최대 성능이 아니라 AI가 신뢰성 있게 관측해야 하는 가장 작은 임무 관련 정보(Task-Relevant Information)를 기준으로 결정해야 합니다.

해상도(Resolution)는 센싱 모달리티(Sensing Modality)에 따라 서로 다른 의미를 가집니다. 카메라(Camera)에서는 일반적으로 픽셀 수(Pixel Count)와 각도 세부도(Angular Detail)가 공간 해상도와 관련됩니다. 라이다(LiDAR)에서는 각도 해상도(Angular Resolution)와 포인트 밀도(Point Density)가 표면을 얼마나 조밀하게 샘플링하는지를 결정합니다. 레이더(Radar)의 해상도는 거리, 각도 또는 속도 방향의 분리 능력을 의미할 수 있으며, 엔코더(Encoder)와 고유수용감각 센서(Proprioceptive Sensor)는 위치, 힘, 토크 등의 상태를 얼마나 세밀하게 양자화(Quantization)하는가로 해상도를 표현합니다. 이러한 값은 각각의 물리적 의미를 고려하지 않고 직접 비교할 수 없습니다.

카메라 해상도(Camera Resolution)는 임무에 중요한 객체가 관련 거리에서 얼마나 많은 유효 픽셀(Useful Pixel)로 표현되는지를 기준으로 평가해야 합니다. 고해상도 이미지라도 원거리 객체가 지나치게 작거나, 모션 블러(Motion Blur)가 세부 정보를 파괴하거나, 광학계가 센서의 명목상 해상도를 유지하지 못한다면 신뢰성 있는 인지를 보장하지 못합니다. 렌즈 특성, 픽셀 크기, 노출(Exposure), 압축(Compression), 시야각(Field of View), 신경망 전처리(Neural Preprocessing)가 함께 AI 모델이 실제로 활용할 수 있는 유효 시각 정보를 결정합니다.

카메라 해상도와 시야각 사이에도 절충 관계(Tradeoff)가 존재합니다. 일정한 수의 픽셀을 더 넓은 시야각에 분배하면 넓은 공간을 관측할 수 있지만 단위 각도 영역당 픽셀 수는 감소합니다. 좁은 시야각은 특정 방향에서 더 세밀한 정보를 제공하지만 주변 상황 인식(Situational Awareness)은 감소합니다. 따라서 다중 카메라 시스템(Multi-Camera System)은 모든 카메라의 성능을 동일하게 최대화하기보다 광각 센싱(Wide-Angle Sensing)으로 주변 상황을 인식하고 좁은 시야 또는 고해상도 센싱으로 원거리나 임무 중요 영역을 관측하도록 구성할 수 있습니다.

라이다 해상도(LiDAR Resolution)는 각도 샘플링(Angular Sampling)과 거리에 크게 영향을 받습니다. 센서 가까이에서는 서로 가깝게 위치한 두 개의 인접 빔(Adjacent Beam)도 거리가 증가하면 빔 사이의 물리적 간격이 점점 커집니다. 따라서 작은 장애물은 근거리에서는 많은 포인트를 생성하지만 장거리에서는 몇 개의 포인트만 생성할 수 있습니다. 특히 후속 AI가 기하학적 분류(Geometric Classification), 점유 추정(Occupancy Estimation), 정밀 표면 재구성(Precise Surface Reconstruction)에 의존한다면 포인트 밀도를 거리와 객체 크기의 함수로 분석해야 합니다.

센서 거리(Sensor Range)는 유용한 정보를 획득할 수 있는 물리적 거리를 결정합니다. 명목상 최대 거리(Nominal Maximum Range)가 반드시 실제 운용 거리(Operational Range)를 의미하지는 않습니다. 일반적으로 거리가 증가하면 탐지 품질이 점진적으로 감소하며 객체 크기, 반사율(Reflectivity), 조명, 대기 조건, 센서 방향, 잡음, 요구 신뢰도(Required Confidence)가 실제 사용 가능한 거리에 영향을 줍니다. 따라서 피지컬 AI 설계에서는 제조사가 제시하는 최대 거리보다 특정 임무를 신뢰성 있게 수행할 수 있는 거리를 정의해야 합니다.

거리 요구사항(Range Requirement)은 로봇 속도와 반응 시간(Reaction Time)에 밀접하게 연결됩니다. 빠른 플랫폼은 인지, 융합(Fusion), 예측(Prediction), 계획(Planning), 제어(Control), 통신, 기계적 제동(Mechanical Braking)에 시간이 필요하므로 더 먼 거리에서 위험 요소를 관측해야 합니다. 로봇이 속도 (v)로 이동하고 센싱-행동 파이프라인(Sensing-to-Action Pipeline)에 지연시간 (T)가 필요하다면 실제 행동이 시작되기 전까지 플랫폼은 대략 (vT)만큼 이동합니다. 여기에 제동 거리, 불확실성, 안전 여유(Safety Margin)를 위한 추가 거리가 필요합니다.

이러한 관계 때문에 고속 피지컬 AI에서 장거리 센싱(Long-Range Sensing)은 단순한 센서 기능이 아니라 아키텍처 요구사항(Architectural Requirement)이 됩니다. 장거리 관측은 더 이른 탐지와 예측을 가능하게 하고, 중거리 센싱(Medium-Range Sensing)은 로컬 계획(Local Planning)과 궤적 정교화(Trajectory Refinement)를 지원합니다. 근거리 센싱(Near-Range Sensing)은 즉각적인 충돌 회피, 도킹(Docking), 조작(Manipulation), 상호작용에 필수적입니다. 따라서 효과적인 센싱 아키텍처는 하나의 최대거리 센서가 모든 공간 요구사항을 해결한다고 가정하기보다 여러 거리 계층(Range Layer)을 결합해야 합니다.

갱신율(Update Rate)은 변화하는 물리적 상태에 대해 센서가 얼마나 자주 새로운 정보를 생성하는지를 결정합니다. 높은 갱신율은 관측 사이의 시간 간격을 줄이고 빠른 움직임의 추적, 제어 응답성(Control Responsiveness), 시간적 상태 추정(Temporal State Estimation)을 개선할 수 있습니다. 그러나 필요한 갱신율은 측정 대상의 동역학(Dynamics)에 따라 달라집니다. 천천히 변화하는 환경 특성이 신체 움직임, 휠 회전, 진동, 고속 객체 움직임과 동일한 샘플링 주파수를 필요로 하는 것은 아닙니다.

서로 다른 모달리티는 본질적으로 서로 다른 시간 척도(Temporal Scale)에서 동작합니다. 카메라는 초당 수십 프레임(Frame per Second)을 제공할 수 있고, 스캐닝 라이다는 비교적 낮은 주기로 전체 공간 관측을 갱신할 수 있으며, 레이더는 상대적으로 빈번한 운동 관련 측정값을 제공할 수 있습니다. IMU, 휠 엔코더(Wheel Encoder), 관절 센서(Joint Sensor)는 빠른 플랫폼 및 액추에이터 동역학을 측정하기 때문에 일반적으로 훨씬 높은 주파수로 동작합니다. 따라서 피지컬 AI 시스템은 모든 센서가 하나의 갱신율을 공유한다고 가정하기보다 비동기 관측(Asynchronous Observation)을 통합해야 합니다.

센서의 명목상 갱신율(Nominal Update Rate)은 유효 정보율(Useful Information Rate)과도 구분해야 합니다. 더 많은 측정값을 생성한다고 해서 각 측정이 항상 의미 있는 새로운 정보를 추가하는 것은 아닙니다. 거의 정적인 장면에서 연속적으로 생성되는 고주파 프레임은 높은 중복성을 가질 수 있는 반면 빠르게 변화하는 환경에서는 조밀한 시간적 샘플링이 필요할 수 있습니다. AI 시스템은 이러한 차이를 활용하여 환경 변화에 따라 적응형 센싱(Adaptive Sensing), 이벤트 기반 처리(Event-Driven Processing), 프레임 선택(Frame Selection), 동적 컴퓨팅 할당(Dynamic Compute Allocation)을 수행할 수 있습니다.

갱신율은 모션 블러와 기하학적 왜곡(Geometric Distortion)에도 직접적인 영향을 줍니다. 카메라의 노출 시간이 길어지면 밝기를 개선할 수 있지만 빠르게 이동하는 객체나 자기 운동(Ego-Motion)에 의해 영상이 흐려질 수 있습니다. 스캐닝 센서는 환경의 서로 다른 부분을 약간 다른 시간에 획득하므로 이동하는 로봇에서는 결과적인 기하 표현이 왜곡될 수 있습니다. 고주파 IMU 측정과 운동 보상(Motion Compensation)을 사용하면 이러한 관측을 정렬하는 데 도움이 되지만 시간적 정확성(Temporal Accuracy)은 여전히 유효 센싱 품질의 근본적인 요소입니다.

따라서 해상도, 거리, 갱신율은 서로 독립적인 설계 파라미터가 아니라 상호 연결되어 있습니다. 프레임 레이트를 유지하면서 카메라 해상도를 높이면 데이터 대역폭(Data Bandwidth)이 크게 증가합니다. 라이다의 각도 밀도(Angular Density)와 스캔 주파수를 높이면 포인트 처리량(Point Throughput)이 증가합니다. 센싱 거리를 확장하려면 더 높은 감도(Sensitivity), 강력한 신호 처리(Signal Processing), 향상된 광학계 또는 추가적인 컴퓨팅이 필요할 수 있습니다. 따라서 센서 사양은 각각을 독립적으로 최대화하기보다 다차원 운용점(Multidimensional Operating Point)으로 평가해야 합니다.

이러한 파라미터는 후속 AI 워크로드(AI Workload)도 결정합니다. 고해상도 이미지는 더 많은 메모리 이동과 더 큰 신경망 입력(Neural Input)을 요구할 수 있습니다. 고밀도 포인트 클라우드는 필터링, 복셀화(Voxelization), 특징 추출(Feature Extraction), 융합 비용을 증가시킵니다. 높은 갱신율은 인지 모델을 더 자주 실행하게 만들고 각 처리 주기에 사용할 수 있는 시간을 감소시킵니다. 따라서 선택된 센서 구성은 요구되는 종단간 지연시간(End-to-End Latency)을 유지하면서 CPU, GPU, 가속기(Accelerator), 메모리, 통신, 열 예산(Thermal Budget)에 적합해야 합니다.

AI 아키텍처(AI Architecture)는 선택적 처리(Selective Processing)를 통해 이러한 비용의 일부를 줄일 수 있습니다. 이미지를 리사이즈(Resize)하거나 크롭(Crop)할 수 있고, 피라미드 표현(Pyramidal Representation)을 이용하여 서로 다른 영역을 서로 다른 스케일로 처리하거나 관심 영역(Region of Interest)에 더 많은 컴퓨팅 자원을 집중할 수 있습니다. 포인트 클라우드는 거리와 임무 관련성에 따라 다운샘플링(Downsampling)하거나 복셀화할 수 있습니다. 시간 모델(Temporal Model)은 모든 정보를 처음부터 다시 계산하지 않고 이전 표현을 재사용하여 센싱 품질과 컴퓨팅 자원을 더욱 지능적으로 배분할 수 있습니다.

그러나 전처리(Preprocessing)는 센서가 처음부터 획득하지 못한 정보를 복원할 수 없습니다. 지나친 이미지 다운샘플링은 작은 객체를 제거할 수 있고, 희소한 라이다 샘플링은 좁은 장애물을 놓칠 수 있으며, 부족한 갱신율에서는 빠른 사건이 두 관측 사이에서 발생할 수 있습니다. 따라서 센서-AI 공동설계(Sensor-AI Co-Design)는 어떤 정보는 안전하게 압축할 수 있고 어떤 정보는 물리적 측정 능력으로 반드시 보존해야 하는지를 결정해야 합니다. 적절한 경계는 임무 위험(Task Risk), 불확실성, 후속 모델의 동작에 따라 달라집니다.

변화하는 운용 조건(Operating Condition)은 적응형 센싱 구성(Adaptive Sensing Configuration)을 정당화할 수 있습니다. 개방된 환경에서 천천히 이동하는 로봇은 혼잡한 교차로에 접근하거나 정밀 도킹을 수행하는 로봇과 동일한 수준의 시간적 처리 강도(Temporal Processing Intensity)를 요구하지 않을 수 있습니다. 카메라는 노출이나 프레임 처리 전략을 변경할 수 있고, 인지 파이프라인은 해상도를 조정할 수 있으며, AI는 불확실성이 높은 영역이나 순간에 더 많은 컴퓨팅 자원을 할당할 수 있습니다. 이러한 적응은 센싱 파라미터를 지능형 자원 관리(Intelligent Resource Management)와 직접 연결합니다.

평가(Evaluation)는 개별 센서 사양보다 후속 임무 성능(Downstream Task Performance)에 초점을 맞추어야 합니다. 후보 구성은 거리, 속도, 조명, 날씨, 객체 크기, 움직임, 가림 조건을 변화시키면서 시험할 수 있습니다. 평가 지표에는 탐지 확률(Detection Probability), 위치 추정 오차(Localization Error), 기하학적 정확도(Geometric Accuracy), 추적 연속성(Tracking Continuity), 상태 추정 불확실성(State-Estimation Uncertainty), 반응 여유(Reaction Margin), AI 추론 지연시간(AI Inference Latency) 등이 포함될 수 있습니다. 이를 통해 해상도, 거리 또는 갱신율의 증가가 실제 시스템 성능 향상으로 연결되는 구간을 식별할 수 있습니다.

안전 여유(Safety Margin)는 특히 중요합니다. 센싱 성능은 사양 한계 부근에서 불안정해질 수 있기 때문입니다. 특정 거리에서 객체를 탐지할 수 있다고 명시된 센서도 불리한 환경 조건에서는 충분한 신뢰도를 제공하지 못할 수 있습니다. 마찬가지로 정상적인 움직임에서는 충분한 갱신율도 급격한 기동에서는 부족할 수 있습니다. 따라서 피지컬 AI 시스템은 이상적인 실험실 성능을 기준으로 하기보다 불확실성 여유를 포함하여 검증된 운용 범위(Validated Operating Envelope)를 기준으로 설계해야 합니다.

따라서 최적의 센싱 구성(Optimal Sensing Configuration)은 최대 해상도, 최장 거리, 최고 갱신율을 모두 결합한 구성이 아닙니다. 이러한 구성은 통신과 컴퓨팅 자원에 과부하를 발생시키고 전력 및 열 요구량을 증가시키며, 그에 비례하는 AI 성능 향상 없이 전체 시스템 효율을 감소시킬 수 있습니다. 목표는 신뢰성 있는 인지, 예측, 계획, 제어를 수행하기 위해 임무 중요 상태를 필요한 거리와 주기로 관측할 수 있을 만큼 충분한 공간적·시간적 정보(Spatial and Temporal Information)를 확보하는 것입니다.

센서 해상도, 거리, 갱신율(Sensor Resolution, Range, and Update Rate)은 공간 커버리지와 중복성(Spatial Coverage and Redundancy)을 이후의 센서 대역폭 및 컴퓨팅 부하(Sensor Bandwidth and Compute Load), 시간 동기화와 타임스탬핑(Time Synchronization and Timestamping), 센서 융합 아키텍처(Sensor Fusion Architecture), 모델 주도 센서 선택(Model-Driven Sensor Selection), 센서 성능 저하 운용(Degraded-Sensor Operation), 정량적 센서-컴퓨팅-대역폭 예산화(Quantitative Sensor-Compute-Bandwidth Budgeting)와 연결합니다. 핵심 원칙은 피지컬 지능(Physical Intelligence)이 실제 행동에 활용할 수 있는 정보를 얻는 곳에 센싱 정밀도(Sensing Fidelity)를 집중하고, 시스템이 효과적으로 활용할 수 없는 불필요한 데이터 생성을 피하는 것입니다.

## 04.07. Sensor Bandwidth and Compute Load

![](images/image9.png){width="7.268055555555556in" height="7.268055555555556in"}

센서 대역폭(Sensor Bandwidth)은 관측 데이터가 센싱 하드웨어(Sensing Hardware)에서 이를 해석하는 컴퓨팅 시스템(Computing System)으로 이동해야 하는 데이터 전송률을 의미합니다. 피지컬 AI(Physical AI)에서는 카메라(Camera), 라이다(LiDAR), 레이더(Radar), 깊이 센서(Depth Sensor), IMU, 고유수용감각 장치(Proprioceptive Device)가 동시에 작동하기 때문에 이러한 데이터 흐름이 매우 커질 수 있습니다. 따라서 대역폭은 단순한 통신 사양이 아니라 유용한 물리적 정보가 요구되는 시간 안에 AI 모델에 도달할 수 있는지를 결정하는 핵심 요소입니다.

센서의 원시 대역폭(Raw Bandwidth)은 해상도(Resolution), 비트 깊이(Bit Depth), 채널 수(Channel Count), 갱신율(Update Rate), 데이터 표현 방식(Data Representation)에 따라 결정됩니다. 높은 프레임 레이트(Frame Rate)로 고해상도 컬러 영상을 생성하는 카메라는 압축하기 전에 초당 수백 메가바이트 이상의 데이터를 생성할 수 있습니다. 여러 카메라를 사용하면 이러한 요구량이 배수로 증가하며, 깊이 이미지, 포인트 클라우드(Point Cloud), 레이더 텐서(Radar Tensor), 고주파 상태 측정값도 병렬 데이터 스트림으로 추가되어 통신 및 메모리 자원을 공유해야 합니다.

카메라 시스템(Camera System)은 센싱 충실도(Sensing Fidelity)와 대역폭 사이의 직접적인 관계를 잘 보여줍니다. 영상의 가로 및 세로 해상도를 높이면 픽셀 수가 증가하고, 높은 비트 깊이는 더 많은 밝기 정보를 보존하며, 높은 프레임 레이트는 초당 더 많은 이미지를 생성합니다. 스테레오(Stereo)와 다중 카메라(Multi-Camera) 아키텍처는 이러한 스트림을 다시 증가시킵니다. 생성된 데이터는 유용한 인지 정보가 되기 전에 인터페이스, 버퍼(Buffer), 메모리, 전처리 단계, 신경망(Neural Network)을 통과해야 합니다.

라이다는 좌표(Coordinate), 강도(Intensity), 타임스탬프(Timestamp), 반사 정보(Return Information), 추가적인 속성으로 구성된 공간 포인트를 출력하기 때문에 카메라와 다른 대역폭 특성을 가집니다. 채널 수, 각도 밀도(Angular Density), 스캔 주파수(Scan Frequency), 다중 반사 수(Number of Returns)를 증가시키면 포인트 처리량(Point Throughput)이 증가합니다. 라이다 데이터는 비압축 다중 카메라 영상보다 작을 수 있지만, 불규칙한 공간 구조를 필터링하고 변환하며 인덱싱(Indexing), 복셀화(Voxelization), 융합(Fusion)해야 하기 때문에 포인트 클라우드 처리는 상당한 메모리 트래픽을 발생시킬 수 있습니다.

레이더 대역폭(Radar Bandwidth)은 신호 처리가 어디에서 수행되는지에 따라 크게 달라집니다. 일부 센서는 압축된 객체 탐지(Object Detection) 또는 트랙(Track)을 출력하지만, 다른 센서는 거리-도플러(Range-Doppler), 거리-각도(Range-Angle), 다차원 레이더 텐서(Multidimensional Radar Tensor)와 같은 더욱 풍부한 표현을 제공합니다. 풍부한 표현은 AI 모델이 활용할 수 있는 정보를 더 많이 보존하지만 통신 및 컴퓨팅 요구량을 크게 증가시킬 수 있습니다. 따라서 센서-AI 공동설계(Sensor-AI Co-Design)에서는 신호 처리를 센서 내부, 엣지 프로세서(Edge Processor), 메인 AI 컴퓨터(Main AI Computer) 중 어디에서 수행할지를 결정해야 합니다.

IMU, 엔코더(Encoder), 관절 센서(Joint Sensor), 기타 고유수용감각 장치는 일반적으로 카메라나 라이다보다 훨씬 작은 데이터 양을 생성하지만 초당 수백 또는 수천 개의 샘플을 생성할 수 있습니다. 전체 대역폭은 상대적으로 작지만 시간적 요구사항(Timing Requirement)은 매우 엄격할 수 있습니다. 이러한 측정값이 지연되거나 불규칙하게 전달되면 전체 데이터 양이 작더라도 상태 추정(State Estimation)과 제어(Control) 성능이 저하될 수 있습니다. 이는 대역폭 용량만 충분하다고 해서 유용한 센싱 성능이 보장되는 것은 아니라는 점을 보여줍니다.

통신 인터페이스(Communication Interface)는 센싱 아키텍처에 실질적인 한계를 설정합니다. 이더넷(Ethernet), 자동차용 이더넷(Automotive Ethernet), USB, MIPI, CAN, PCIe 등의 링크는 처리량(Throughput), 지연시간(Latency), 결정성(Determinism), 토폴로지(Topology), 케이블 길이, 동기화 기능, 강건성(Robustness) 측면에서 서로 다릅니다. 프로토콜 오버헤드(Protocol Overhead), 패킷화(Packetization), 경쟁 트래픽, 재전송, 버퍼링, 시스템 수준의 안전 여유가 이론적인 용량의 일부를 사용하기 때문에 명목상 링크 속도를 모두 센서 대역폭으로 사용할 수 있다고 가정해서는 안 됩니다.

따라서 대역폭은 전체 데이터 경로(Complete Data Path)를 기준으로 예산화해야 합니다. 센서 스트림이 인터페이스까지 성공적으로 도달하더라도 시스템 메모리(System Memory)로 복사되거나 GPU 메모리로 전송되고, 전처리 커널(Preprocessing Kernel)에 의해 변환되거나 다른 프로세스와 자원을 공유하는 과정에서 병목(Bottleneck)이 발생할 수 있습니다. 실제 센싱 파이프라인은 센서 링크, 스위치(Switch), 버스(Bus), CPU 메모리, GPU 인터커넥트(GPU Interconnect), 저장장치(Storage), 네트워크 통신을 포함하며, 어느 단계에서든 병목이 발생하면 지연시간이 증가하거나 정보가 손실될 수 있습니다.

컴퓨팅 부하(Compute Load)는 메인 AI 모델이 실행되기 전부터 발생합니다. 카메라 데이터에는 디코딩(Decoding), 보정(Rectification), 크기 조정(Resizing), 정규화(Normalization), 왜곡 보정(Undistortion), 색상 변환(Color Conversion)이 필요할 수 있습니다. 라이다는 필터링, 좌표 변환(Coordinate Transformation), 운동 보상(Motion Compensation), 복셀화, 투영(Projection)이 필요합니다. 레이더는 신호 처리와 텐서 구성이 필요할 수 있습니다. 이러한 연산은 CPU, GPU, 가속기(Accelerator), 메모리 자원을 소비하므로 센서 구성 자체가 신경망 추론 이전의 전처리 비용을 결정합니다.

이후 신경망 추론(Neural Inference)이 또 하나의 주요 컴퓨팅 계층을 추가합니다. 높은 영상 해상도는 비전 모델(Vision Model)이 처리해야 하는 픽셀이나 토큰(Token)의 수를 증가시키고, 더 조밀한 포인트 클라우드는 기하학적 특징 추출(Geometric Feature Extraction) 비용을 증가시키며, 풍부한 다중모달 표현(Multimodal Representation)은 융합 복잡도를 증가시킵니다. 시간 모델(Temporal Model)은 여러 시간 단계의 정보를 유지하고 처리해야 합니다. 따라서 센서 부하는 백본(Backbone), 융합, 월드 모델(World Model), 예측, 계획, 제어 워크로드와 독립적으로 평가할 수 없습니다.

메모리 대역폭(Memory Bandwidth)은 산술 연산 처리량(Arithmetic Throughput)만큼 중요해질 수 있습니다. AI 가속기가 막대한 계산 능력을 제공하더라도 대용량 센서 텐서를 여러 메모리 계층 사이에서 반복적으로 이동해야 한다면 성능이 제한될 수 있습니다. 다중 카메라 특징(Multi-Camera Feature), 복셀 그리드(Voxel Grid), 점유 표현(Occupancy Representation), 시간 이력(Temporal History), 신경망 중간 활성값(Intermediate Neural Activation)은 상당한 메모리 대역폭을 사용할 수 있습니다. 따라서 효율적인 피지컬 AI에서는 명목상 TOPS, FLOPS, GPU 사용률뿐만 아니라 데이터 이동(Data Movement)도 함께 고려해야 합니다.

지연시간은 대역폭과 컴퓨팅 부하를 실제 물리적 행동(Physical Behavior)과 연결합니다. 통신이나 추론 자원이 과부하되어 센서 데이터가 대기열(Queue)에서 기다리게 되면 AI는 더 이상 현재 환경을 정확하게 나타내지 않는 오래된 관측값을 기반으로 의사결정을 수행할 수 있습니다. 높은 처리량도 센싱-행동 마감시간(Sensing-to-Action Deadline) 안에 정보를 처리할 수 있을 때만 의미가 있습니다. 따라서 피지컬 AI 시스템은 평균 처리량만 최적화하기보다 대기열 깊이(Queue Depth), 스케줄링(Scheduling), 버퍼링, 추론 시간, 최악 조건 지연시간(Worst-Case Latency)을 관리해야 합니다.

센서를 추가하면 컴퓨팅 요구량이 비선형적으로 증가할 수도 있습니다. 두 번째 카메라를 추가한다고 해서 항상 처리량이 단순히 두 배가 되는 것은 아닙니다. 카메라 간 연관(Cross-Camera Association), 기하학적 투영(Geometric Projection), 어텐션(Attention), 융합, 시간 정렬(Temporal Alignment)과 같은 추가적인 상호작용이 필요할 수 있기 때문입니다. 다른 모달리티를 추가하는 경우에도 캘리브레이션, 동기화, 표현 변환(Representation Conversion), 융합 연산이 증가합니다. 따라서 센서 수는 개별 센서의 워크로드를 단순히 합산하는 것이 아니라 전체 시스템 그래프(System Graph)에 미치는 영향을 기준으로 평가해야 합니다.

압축(Compression)은 통신 부하를 감소시킬 수 있지만 절충 관계를 발생시킵니다. 무손실 압축(Lossless Compression)은 정보를 보존하지만 일부 센서 스트림에서는 압축률이 제한될 수 있으며, 손실 압축(Lossy Compression)은 이미지나 포인트 클라우드의 대역폭을 크게 줄일 수 있지만 임무에 중요한 세부 정보를 제거할 위험이 있습니다. 압축과 압축 해제(Decompression) 자체도 컴퓨팅 자원을 사용하고 지연시간을 추가합니다. 적절한 전략은 정보 손실이 탐지, 위치 추정(Localization), 추적(Tracking), 월드 모델링, 안전 중요 의사결정에 영향을 미치는지에 따라 결정해야 합니다.

엣지 전처리(Edge Preprocessing)는 원시 측정값을 센서 근처에서 특징(Feature), 탐지 결과, 트랙, 압축된 표현으로 변환하여 중앙 시스템의 대역폭 요구량을 줄일 수 있습니다. 이는 분산 센싱 아키텍처(Distributed Sensing Architecture)의 확장성을 높일 수 있지만 초기 처리 단계에서 미래의 AI 모델이나 교차 모달 융합(Cross-Modal Fusion)에 필요한 정보를 제거할 수도 있습니다. 원시 데이터를 전송하면 유연성을 보존할 수 있지만 통신 및 컴퓨팅 요구량이 증가합니다. 따라서 센서 측, 엣지, 중앙 처리 사이의 기능 분할은 중요한 공동설계 결정입니다.

선택적 처리(Selective Processing)는 워크로드를 제어하는 또 다른 방법입니다. AI는 일부 카메라를 전체 해상도로 처리하면서 중요도가 낮은 시야의 해상도나 프레임 레이트를 줄일 수 있습니다. 관심 영역(Region of Interest)에 더 많은 컴퓨팅 자원을 할당하고, 원거리 영역에는 다른 특징 스케일(Feature Scale)을 적용하며, 정적인 장면에서는 처리 빈도를 낮출 수 있습니다. 따라서 모든 관측을 동일하게 처리하기보다 임무 관련성(Task Relevance), 불확실성(Uncertainty), 움직임, 위험, 사용 가능한 컴퓨팅 자원을 기준으로 센서 데이터의 우선순위를 결정할 수 있습니다.

적응형 컴퓨팅 스케줄링(Adaptive Compute Scheduling)은 이러한 원리를 시간적으로 확장합니다. 개방된 환경에서 저속으로 이동하는 것과 같은 단순한 운용 상황에서는 인지 스택(Perception Stack)의 일부를 낮은 주파수나 낮은 충실도로 실행할 수 있습니다. 불확실성, 속도, 교통량, 지형 복잡도, 상호작용 위험이 증가하면 추가적인 센싱과 컴퓨팅을 활성화할 수 있습니다. 이러한 동적 할당(Dynamic Allocation)은 피지컬 AI가 환경 복잡도를 자원 소비와 직접 연결하면서 높은 부하가 필요한 상황에 컴퓨팅 여유를 보존할 수 있도록 합니다.

다중모달 융합(Multimodal Fusion)은 센서 스트림마다 데이터 크기, 갱신율, 처리 비용이 서로 다르기 때문에 세심한 자원 할당(Resource Allocation)을 필요로 합니다. 카메라 특징은 GPU 계산을 크게 사용할 수 있고, 라이다는 공간 변환(Spatial Transformation)이 필요하며, 레이더는 시간적 운동 정보를 추가하고, IMU 또는 고유수용감각은 고주파 상태 갱신을 요구할 수 있습니다. 융합 아키텍처(Fusion Architecture)는 이러한 스트림이 어디에서 결합되는지를 결정하므로 메모리 사용량, 통신 트래픽, 스케줄링 복잡도, 종단간 지연시간에 큰 영향을 줍니다.

컴퓨팅 과부하(Compute Overload)는 하나의 센싱 고장 모드(Sensing Failure Mode)로 취급해야 합니다. 프로세서가 데이터 처리 속도를 따라가지 못하면 프레임이 누락되거나 측정값이 지연되고, 버퍼가 증가하거나 추론 주기가 건너뛰어질 수 있습니다. 센서 자체는 정상적으로 작동하고 있더라도 정보가 너무 늦게 도착하면 AI는 실질적으로 관측 가능성(Observability)을 잃게 됩니다. 따라서 자원 모니터링(Resource Monitoring)은 프로세서 사용률뿐만 아니라 데이터 연령(Data Age), 누락된 측정값, 대기열 깊이, 추론 마감시간(Inference Deadline), 남아 있는 컴퓨팅 여유(Compute Margin)를 함께 추적해야 합니다.

대역폭이나 컴퓨팅 자원이 제한될 때 성능 저하 운용(Degraded Operation)을 통해 시스템을 보호할 수 있습니다. 로봇은 카메라 해상도를 낮추거나 선택된 처리 주기를 감소시키고, 필수적이지 않은 모델을 비활성화하거나 안전 중요 센서를 우선 처리하며, 속도를 낮추거나 계획을 단순화할 수 있습니다. 이러한 동작은 운영체제의 통제되지 않은 스케줄링에 맡기기보다 명시적으로 설계해야 합니다. 점진적 성능 저하(Graceful Degradation)는 컴퓨팅 과부하 상황에서 갑작스럽게 실패하는 대신 필수적인 인지와 제어 기능을 유지하도록 합니다.

정량적 예산화(Quantitative Budgeting)에서는 각각의 센싱 파이프라인에 대해 데이터 전송률(Data Rate), 전처리 비용, 추론 비용, 메모리 사용량, 지연시간, 전력(Power), 열 부하(Thermal Load)를 추정해야 합니다. 이러한 추정값은 실제 목표 하드웨어(Target Hardware)에서 대표적인 워크로드를 프로파일링(Profiling)하여 검증할 수 있습니다. 실시간 시스템에서는 여러 센서의 데이터가 동시에 집중되거나 복잡한 장면에서 부하가 증가할 수 있기 때문에 평균값만으로는 충분하지 않습니다. 따라서 설계 여유(Design Margin)는 정상 운용뿐만 아니라 최악 조건 또는 높은 백분위수(High-Percentile)의 자원 요구량도 고려해야 합니다.

목표는 센서 대역폭이나 컴퓨팅 부하를 각각 최소화하는 것이 아닙니다. 지나치게 줄이면 신뢰성 있는 인지에 필요한 정보가 손실될 수 있으며, 과도한 센싱은 임무 성능을 향상시키지 못하면서 통신 및 처리 자원을 압도할 수 있습니다. 적절한 운용점(Operating Point)은 로봇의 의도된 운용 범위(Operating Envelope) 전체에서 지연시간, 전력, 열, 메모리, 신뢰성 제약을 유지하면서 인지, 예측, 계획, 제어에 충분한 정보를 제공해야 합니다.

따라서 센서 대역폭과 컴퓨팅 부하(Sensor Bandwidth and Compute Load)는 센서 해상도·거리·갱신율(Sensor Resolution, Range, and Update Rate)을 이후의 시간 동기화와 타임스탬핑(Time Synchronization and Timestamping), 센서 융합 아키텍처(Sensor Fusion Architecture), 모델 주도 센서 선택(Model-Driven Sensor Selection), 센서 성능 저하 운용(Degraded-Sensor Operation), 정량적 센서-컴퓨팅-대역폭 예산화(Quantitative Sensor-Compute-Bandwidth Budgeting)와 연결합니다. 피지컬 AI는 센싱과 컴퓨팅을 독립된 하드웨어 및 소프트웨어 하위 시스템이 아니라 자원 제약을 가진 하나의 통합 정보 파이프라인(Resource-Constrained Information Pipeline)으로 다룰 때 실질적으로 구현될 수 있습니다.

## 04.08. Time Synchronization and Timestamping

![](images/image10.png){width="7.268055555555556in" height="7.268055555555556in"}

시간 동기화(Time Synchronization)는 다중모달 센서(Multimodal Sensor)가 서로 다른 속도와 데이터 획득 파이프라인(Acquisition Pipeline)을 통해 지속적으로 변화하는 세계를 관측하기 때문에 피지컬 AI(Physical AI)에서 필수적입니다. 카메라(Camera) 이미지, 라이다(LiDAR) 스캔, 레이더(Radar) 측정, IMU 샘플, 휠 엔코더(Wheel Encoder) 측정값이 컴퓨터에는 동시에 도착하더라도 실제로는 서로 다른 물리적 시점을 나타낼 수 있습니다. 신뢰성 있는 시간 정보가 없다면 센서 융합(Sensor Fusion)은 실제로 동시에 존재하지 않았던 관측값을 결합할 수 있습니다.

타임스탬핑(Timestamping)은 각각의 측정값에 시간 기준(Time Reference)을 부여하여 시스템이 해당 물리적 사건이 언제 관측되었는지를 판단할 수 있도록 합니다. 타임스탬프(Timestamp)는 소프트웨어가 데이터를 수신한 시간을 단순히 기록하기보다 실제 측정 사건(Measurement Event)이 발생한 시점을 가능한 한 정확하게 나타내야 합니다. 센서 데이터 획득, 내부 처리, 전송, 버퍼링(Buffering), 드라이버(Driver), 운영체제 스케줄링(Operating-System Scheduling)에서 가변적인 지연이 발생할 수 있기 때문에 이러한 차이는 매우 중요합니다.

공통 클록(Common Clock)은 분산된 장치에서 생성된 측정값을 비교하기 위해 필요한 시간 좌표계(Temporal Coordinate System)를 제공합니다. 센서와 컴퓨팅 노드는 각각 독립적인 발진기(Oscillator)를 사용할 수 있으며 이들의 클록은 오프셋(Offset)과 주파수에서 차이가 발생할 수 있습니다. 처음에는 클록이 일치하더라도 발진기의 불완전성으로 인해 시간이 지나면서 클록 드리프트(Clock Drift)가 발생합니다. 동기화 메커니즘은 이러한 클록을 주기적으로 정렬하여 서로 다른 장치가 생성한 타임스탬프를 로봇 운용 전체에서 비교 가능하도록 유지합니다.

하드웨어 동기화(Hardware Synchronization)는 공통 트리거(Common Trigger), 펄스(Pulse), 클록 신호(Clock Signal)를 센싱 장치에 직접 분배하여 높은 시간적 일관성(Temporal Consistency)을 제공할 수 있습니다. 카메라는 동일한 트리거를 기준으로 프레임을 획득할 수 있으며 다른 센서도 공유된 시간 소스를 참조할 수 있습니다. 하드웨어 동기화는 비결정적인 소프트웨어 지연에 대한 의존성을 감소시키므로 고속 운동, 정밀 위치 추정(Precise Localization), 다중 카메라 인지(Multi-Camera Perception), 기하학적 재구성(Geometric Reconstruction), 긴밀하게 결합된 센서 융합에 특히 중요합니다.

소프트웨어 동기화(Software Synchronization)는 일반적으로 구현하기 쉽지만 더 큰 시간적 불확실성을 가집니다. 네트워크 프로토콜(Network Protocol)이나 호스트 소프트웨어(Host Software)는 전용 트리거 배선 없이 클록 오프셋을 추정하고 타임스탬프를 정렬할 수 있습니다. 이는 상대적으로 느리거나 시간 민감도가 낮은 기능에는 충분할 수 있지만 네트워크 혼잡(Network Congestion), 스케줄링 지터(Scheduling Jitter), 패킷 지연(Packet Delay), 운영체제 동작으로 인해 변동성이 발생할 수 있습니다. 따라서 필요한 동기화 방식은 후속 AI가 요구하는 시간 정확도(Temporal Accuracy)를 기준으로 결정해야 합니다.

정밀 시간 프로토콜(Precision Time Protocol, PTP)은 분산 로봇 및 산업 시스템에서 이더넷(Ethernet) 네트워크를 통해 클록을 동기화하기 위해 널리 사용되며 일반적인 네트워크 시간 동기화보다 훨씬 높은 정밀도를 제공할 수 있습니다. 네트워크 인터페이스, 스위치(Switch), 센서, 하드웨어 타임스탬핑(Hardware Timestamping)이 이를 지원하면 PTP를 통해 여러 장치에 공통 시간 기준(Common Time Base)을 구축할 수 있습니다. 그러나 실제 동기화 정확도는 단순히 프로토콜 이름만으로 결정되는 것이 아니라 전체 네트워크 아키텍처에 따라 달라집니다.

하드웨어 타임스탬핑은 응용 프로그램 수준의 소프트웨어에 의존하지 않고 네트워크 인터페이스에 가까운 위치에서 패킷의 시간을 기록하여 동기화 정확도를 향상시킵니다. 소프트웨어 타임스탬프(Software Timestamp)에는 장치 드라이버, 대기열(Queue), 커널 스케줄링(Kernel Scheduling), 처리 과정에서 발생하는 예측하기 어려운 지연이 포함될 수 있습니다. 타임스탬프 생성 위치를 실제 물리적 데이터 획득 또는 전송 사건에 가깝게 이동시키면 이러한 불확실성을 줄이고 상태 추정(State Estimation)과 융합 알고리즘이 센서의 시간 관계를 더욱 정확하게 처리할 수 있습니다.

센서 데이터 획득(Sensor Acquisition) 방식 자체도 타임스탬프 해석을 복잡하게 만들 수 있습니다. 글로벌 셔터 카메라(Global-Shutter Camera)는 전체 프레임을 거의 동시에 노출하지만 롤링 셔터 카메라(Rolling-Shutter Camera)는 이미지의 서로 다른 행(Row)을 서로 다른 시간에 기록합니다. 회전식 라이다(Rotating LiDAR)는 하나의 스캔 동안 포인트를 순차적으로 수집하기 때문에 완성된 포인트 클라우드(Point Cloud)가 정확히 하나의 순간을 나타내지 않습니다. 레이더 처리 역시 순간적인 관측을 생성하기보다 특정 측정 구간 동안 신호를 통합할 수 있습니다.

따라서 전체 센서 프레임에 하나의 타임스탬프만 부여하는 것으로 충분하지 않은 경우가 있습니다. 플랫폼이나 객체의 움직임이 큰 경우 개별 라이다 포인트, 카메라 행, 레이더 측정값 또는 다른 샘플에 더욱 세밀한 시간 정보가 필요할 수 있습니다. 피지컬 AI 시스템은 타임스탬프가 노출 시작(Exposure Start), 노출 중심(Exposure Center), 스캔 시작(Scan Start), 패킷 전송(Packet Transmission), 측정 완료(Measurement Completion), 소프트웨어 도착(Software Arrival) 중 어느 시점을 나타내는지 이해해야 합니다. 이러한 시점 사이에는 실제 시스템에서 의미 있는 차이가 발생할 수 있습니다.

IMU는 상대적으로 작은 데이터 스트림에서도 고품질 시간 정보가 중요한 이유를 잘 보여줍니다. IMU 측정값은 일반적으로 상대적으로 낮은 주파수의 카메라나 라이다 관측 사이에서 로봇 상태를 전파(State Propagation)하는 데 사용됩니다. IMU 타임스탬프가 이동되어 있거나 불규칙하면 환경 측정값에 적용되는 회전과 이동 추정이 잘못됩니다. 따라서 작은 시간 오차도 빠른 회전, 가속, 진동, 고속 이동 상황에서는 상당한 공간 오차(Spatial Error)로 변환될 수 있습니다.

시간 동기화는 공간 캘리브레이션(Spatial Calibration)과 밀접하게 연결됩니다. 외부 캘리브레이션(Extrinsic Calibration)은 센서들이 서로 어디에 위치하는지를 정의하는 반면, 시간 캘리브레이션(Temporal Calibration)은 각각의 측정값이 언제 서로 대응하는지를 정의합니다. 로봇이 움직이는 상황에서는 완벽한 기하학적 캘리브레이션만으로 상당한 시간 오차를 보상할 수 없습니다. 반대로 정확한 타임스탬프만으로 잘못된 센서 자세(Sensor Pose)를 보정할 수도 없습니다. 따라서 신뢰성 있는 다중모달 융합에는 공간 및 시간 캘리브레이션이 동시에 유효하게 유지되어야 합니다.

운동 보상(Motion Compensation)은 정확한 시간 정보를 이용하여 서로 다른 순간에 획득된 측정값을 하나의 공통 기준 시점(Common Reference Time)으로 변환합니다. 예를 들어 플랫폼이 움직이는 동안 수집된 라이다 포인트는 IMU 또는 오도메트리(Odometry) 추정값을 이용하여 디스큐(Deskew)할 수 있습니다. 카메라, 레이더, 포인트 클라우드 관측값도 자기 운동(Ego-Motion)에 따라 변환할 수 있습니다. 이러한 연산은 신뢰할 수 있는 타임스탬프에 의존하며 시간 관계가 잘못되면 공간 변환도 잘못됩니다.

센서가 비동기적으로 동작할 때는 버퍼링이 필요합니다. 융합 알고리즘은 고주파 IMU 스트림, 다른 주파수의 카메라 프레임, 더 낮은 주파수의 라이다 스캔, 또 다른 주기의 레이더 관측값을 수신할 수 있습니다. 시스템은 모든 데이터가 동시에 도착하도록 요구하는 대신 타임스탬프가 부여된 버퍼(Timestamped Buffer)에 측정값을 저장하고 원하는 융합 시점(Fusion Time)에 대응하는 관측값을 검색할 수 있습니다. 따라서 버퍼 설계(Buffer Design)는 시간적 센서 아키텍처(Temporal Sensor Architecture)의 일부가 됩니다.

보간(Interpolation)은 센서가 정확한 시점에 측정값을 생성하지 않았을 때 해당 시점의 상태를 추정하도록 합니다. 필요한 경우 인접한 샘플 사이에서 엔코더 위치, 자세(Pose), 속도 또는 관성 상태를 보간할 수 있습니다. 더욱 정교한 추정기(Estimator)는 운동 모델(Motion Model)을 이용하여 동적 상태를 전파합니다. 보간은 시간적 불일치(Temporal Mismatch)를 줄일 수 있지만 지나치게 느린 센싱, 긴 통신 지연, 부정확한 타임스탬프로 인한 정보 손실까지 제거할 수는 없습니다.

지연시간(Latency)과 동기화(Synchronization)는 서로 혼동해서는 안 됩니다. 두 센서가 매우 정확하게 동기화되어 있더라도 한 센서의 처리 지연이 다른 센서보다 훨씬 길 수 있습니다. 타임스탬프가 실제 데이터 획득 시점을 정확히 나타낸다면 측정값이 늦게 도착하더라도 시스템은 두 측정값 사이의 시간 관계를 파악할 수 있습니다. 지연시간은 정보가 사용 가능해졌을 때 얼마나 오래된 정보인지를 결정하고, 동기화는 해당 정보가 언제 발생했는지를 시스템이 얼마나 정확하게 알고 있는지를 결정합니다.

지터(Jitter)는 타이밍 또는 지연시간의 변동을 의미하며 일정한 지연보다 관리하기 어려울 수 있습니다. 고정된 오프셋은 측정하여 보상할 수 있지만 예측할 수 없는 지연 변화는 시간 정렬(Temporal Alignment)에 불확실성을 발생시킵니다. 네트워크 경쟁(Network Contention), 운영체제 스케줄링, 버퍼링, 센서 펌웨어(Sensor Firmware), 가변적인 처리 워크로드가 지터를 발생시킬 수 있습니다. 따라서 실시간 피지컬 AI에서는 평균 지연시간만이 아니라 시간 분포(Timing Distribution)와 최악 조건 동작(Worst-Case Behavior)을 평가해야 합니다.

분산 컴퓨팅 아키텍처(Distributed Compute Architecture)는 센싱, 전처리, 추론, 제어가 서로 다른 프로세서에서 수행될 수 있기 때문에 동기화를 더욱 어렵게 만듭니다. 엣지 컴퓨터(Edge Computer), 마이크로컨트롤러(Microcontroller), GPU, 센서 프로세서(Sensor Processor), 중앙 컴퓨터(Central Computer)는 상태 정보를 교환할 때 일관된 시간 기준을 사용해야 합니다. 그렇지 않으면 각각의 하위 시스템은 개별적으로 올바르게 작동하더라도 데이터가 서로 다른 현재 시간(Current Time)을 기준으로 해석되어 전체 시스템에서는 불일치하는 동작이 발생할 수 있습니다.

AI 모델 역시 시간적 정확성에 의존합니다. 시간 트랜스포머(Temporal Transformer), 순환 신경망(Recurrent Network), 추적 시스템(Tracking System), 점유 모델(Occupancy Model), 월드 모델(World Model)은 관측 시퀀스(Observation Sequence)를 통해 움직임과 상태 전이(State Transition)를 추론합니다. 잘못된 시간 정보는 객체의 겉보기 속도와 가속도를 변화시키고 학습된 시간적 관계를 왜곡할 수 있습니다. 따라서 타임스탬프 품질은 전통적인 상태 추정뿐만 아니라 현대 피지컬 AI 모델이 학습하는 시간 표현(Temporal Representation)의 유효성에도 영향을 줍니다.

데이터셋 수집(Dataset Collection)에도 동일한 원칙이 적용됩니다. 모델이 물리적 동역학(Physical Dynamics)을 학습하려면 카메라, 라이다, 레이더, IMU, 행동(Action), 로봇 상태(Robot State)의 학습 데이터가 신뢰성 있는 시간 관계를 유지해야 합니다. 데이터셋에 포함된 동기화 오차는 모델에게 일관되지 않은 움직임이나 잘못된 행동 결과(Action Consequence)로 나타날 수 있습니다. 따라서 시간 캘리브레이션은 데이터 품질(Data Quality)의 일부이며 데이터 수집이 완료된 이후에도 장기간 모델 성능에 영향을 미칠 수 있습니다.

동기화 실패(Synchronization Failure)는 운용 중에 탐지할 수 있어야 합니다. 시스템은 클록 오프셋, 드리프트, 누락된 타임스탬프, 비정상적인 데이터 도착 간격(Inter-Arrival Interval), 동기화 상태, 측정 데이터 연령(Measurement Age)을 모니터링할 수 있습니다. 시간 불확실성이 허용 가능한 범위를 초과하면 융합 알고리즘은 해당 측정값을 거부하거나 불확실성을 증가시키고, 영향을 받은 센서에 대한 의존도를 낮추거나 성능 저하 운용(Degraded Operation)으로 전환할 수 있습니다. 모니터링 없이 타임스탬프를 무조건 신뢰하면 진단하기 어려운 미묘한 고장이 발생할 수 있습니다.

필요한 동기화 정확도(Synchronization Accuracy)는 임의로 선택하기보다 물리적 동역학을 기준으로 도출해야 합니다. 천천히 움직이는 실내 로봇은 고속 차량, 민첩한 4족 보행 로봇(Agile Quadruped), 드론(Drone), 정밀 매니퓰레이터(Precision Manipulator)보다 더 큰 시간 오차를 허용할 수 있습니다. 객체나 로봇이 빠르게 움직이면 불과 수 밀리초(Millisecond)의 오차도 의미 있는 공간 변위(Spatial Displacement)로 이어질 수 있습니다. 따라서 시간 오차 예산(Temporal Error Budget)은 결과적으로 발생하는 상태 추정 및 제어 오차로 변환하여 평가해야 합니다.

검증(Validation)은 센서 데이터 획득에서 통신 및 처리에 이르는 전체 시간 흐름을 측정할 수 있습니다. 엔지니어는 트리거 신호(Trigger Signal), 하드웨어 타임스탬프, 네트워크 타이밍(Network Timing), 기록된 센서 시퀀스, 관측된 움직임을 비교하여 오프셋, 드리프트, 지터, 지연시간을 추정할 수 있습니다. 여러 센서와 AI 모델이 동시에 작동할 때의 타이밍 동작은 시스템이 유휴 상태일 때와 크게 다를 수 있으므로 실제 네트워크 및 컴퓨팅 부하를 포함한 조건에서 시험해야 합니다.

따라서 강건한 타이밍 아키텍처(Robust Timing Architecture)는 적절한 공통 클록, 신뢰성 있는 타임스탬프 생성, 명확하게 정의된 센서 데이터 획득 시점(Sensor Acquisition Semantics), 동기화 모니터링, 타임스탬프 기반 버퍼링, 보간, 운동 보상을 결합합니다. 목표는 단순히 여러 장치가 동일한 클록 값을 표시하도록 만드는 것이 아닙니다. 인지(Perception), 위치 추정(Localization), 융합, 예측(Prediction), 계획(Planning), 제어에 필요한 물리적 시간 관계를 정확하게 유지하는 것이 핵심입니다.

따라서 시간 동기화와 타임스탬핑(Time Synchronization and Timestamping)은 센서-AI 공동설계(Sensor-AI Co-Design)의 시간적 기반(Temporal Foundation)을 형성합니다. 대역폭과 컴퓨팅이 센서 정보를 충분히 빠르게 이동시키고 처리할 수 있는지를 결정한다면, 동기화는 각각의 관측값을 물리적 시간축(Physical Timeline)의 올바른 위치에 배치할 수 있는지를 결정합니다. 신뢰성 있는 피지컬 AI를 구현하려면 모든 중요한 측정값이 충분한 정확도로 두 가지 질문에 답할 수 있어야 합니다. 무엇을 관측했는가, 그리고 언제 관측했는가.

## 04.09. Sensor Fusion Architecture

![](images/image11.png){width="7.268055555555556in" height="7.268055555555556in"}

센서 융합 아키텍처(Sensor Fusion Architecture)는 여러 센싱 모달리티(Sensing Modality)의 관측값을 결합하여 로봇과 주변 환경에 대한 일관된 표현(Coherent Representation)을 만드는 방법을 결정합니다. 카메라(Camera), 라이다(LiDAR), 레이더(Radar), IMU, 엔코더(Encoder), 고유수용감각 센서(Proprioceptive Sensor)는 서로 보완적이지만 불완전한 정보를 제공합니다. 따라서 센서 융합은 단순한 측정값의 집합이 아니라 이질적인 관측값을 인지(Perception), 예측(Prediction), 계획(Planning), 제어(Control)에 활용할 수 있는 일관된 상태로 변환하는 피지컬 AI(Physical AI)의 아키텍처 과정입니다.

서로 다른 센서는 동일한 물리 세계의 서로 다른 특성을 설명합니다. 카메라는 풍부한 외형 및 의미 정보(Semantic Information)를 제공하고, 라이다는 명시적인 기하학적 구조(Geometric Structure)를 제공하며, 레이더는 거리 및 운동 정보를 제공합니다. IMU와 고유수용감각(Proprioception)은 플랫폼의 동역학과 내부 상태를 설명합니다. 어떤 모달리티도 모든 조건에서 완전한 정보를 제공하지 못합니다. 융합은 각 센서의 상호 보완적인 장점을 활용하면서 개별 센싱 스트림만으로는 남게 되는 모호성과 불확실성(Uncertainty)을 감소시킵니다.

융합 아키텍처는 먼저 어떤 정보를 결합할 것인지를 정의해야 합니다. 원시 측정값(Raw Measurement), 기하학적 기본 요소(Geometric Primitive), 신경망 특징(Neural Feature), 탐지된 객체, 트랙(Track), 점유 상태(Occupancy State), 추정 자세(Estimated Pose) 등이 모두 융합 입력으로 사용될 수 있습니다. 선택된 표현(Representation)은 원래 정보가 얼마나 많이 유지되는지와 정렬(Alignment)이 얼마나 어려워지는지를 결정합니다. 따라서 융합 아키텍처는 후속 피지컬 AI 모델에서 사용하는 표현 아키텍처(Representation Architecture)와 밀접하게 연결됩니다.

초기 융합(Early Fusion)은 비교적 측정 단계에 가까운 위치에서 센서 정보를 결합합니다. 카메라 픽셀을 깊이 정보나 투영된 라이다 포인트와 연관시키거나, 여러 센서 측정값을 충분한 독립적 해석이 이루어지기 전에 공통 공간 표현(Shared Spatial Representation)으로 변환할 수 있습니다. 초기 융합은 세밀한 교차 모달 관계(Cross-Modal Relationship)를 보존하고 모델이 풍부한 상호작용을 학습하도록 할 수 있지만 정확한 캘리브레이션(Calibration), 동기화(Synchronization), 그리고 상당한 통신 및 컴퓨팅 자원을 요구합니다.

특징 수준 융합(Feature-Level Fusion)은 원시 측정값 대신 학습되거나 설계된 표현을 결합합니다. 각 모달리티를 적절한 인코더(Encoder)로 먼저 처리한 다음 카메라, 라이다, 레이더 또는 고유수용감각 특징을 정렬하여 통합합니다. 이 방식은 원시 데이터의 차원(Dimensionality)을 줄이면서 결정 수준 융합보다 더 많은 정보를 유지합니다. 특징 공간(Feature Space)을 후속 임무에 맞추어 공동으로 최적화할 수 있기 때문에 현대의 다중모달 신경망 아키텍처(Multimodal Neural Architecture)에서 자주 사용됩니다.

후기 융합(Late Fusion)은 객체 탐지 결과(Object Detection), 의미 레이블(Semantic Label), 트랙, 자세, 신뢰도 추정값(Confidence Estimate)과 같은 상위 수준 출력을 결합합니다. 각각의 센싱 파이프라인이 상당한 독립 처리를 수행한 이후 결과를 통합하는 방식입니다. 이러한 아키텍처는 모듈화(Modularity)가 쉽고 컴퓨팅 부하를 관리하기 쉬우며 개별 센서 변경에 강건할 수 있습니다. 그러나 독립적인 인지 파이프라인에서 이미 제거된 정보는 이후 복구할 수 없으므로 미세한 교차 모달 관계를 활용하는 능력이 제한될 수 있습니다.

하이브리드 융합(Hybrid Fusion)은 하나의 융합 수준만 선택하지 않고 여러 단계에서 정보를 결합합니다. 일부 모달리티에는 저수준 기하 정렬(Low-Level Geometric Alignment)을 적용하고, 학습 표현에는 특징 융합을 사용하며, 독립적인 안전 채널에는 결정 융합(Decision Fusion)을 사용할 수 있습니다. 이러한 아키텍처는 복잡도를 제어하면서 유용한 정보를 보존할 수 있습니다. 위치 추정(Localization), 인지, 월드 모델링(World Modeling), 안전 기능은 서로 다른 융합 요구사항을 가지므로 피지컬 AI 시스템에서는 하이브리드 접근법이 유용할 수 있습니다.

공간 정렬(Spatial Alignment)은 융합 수준과 관계없이 기본적으로 필요합니다. 서로 다른 센서의 측정값은 서로 다른 좌표계(Coordinate Frame)에서 생성되기 때문에 일관된 공간 기준으로 변환해야 합니다. 외부 캘리브레이션(Extrinsic Calibration)은 이러한 좌표계 사이의 관계를 정의하는 기하학적 변환을 제공합니다. 캘리브레이션이 부정확하거나 장착 기하 구조가 변하거나 변환이 잘못 적용되면 융합 오차가 발생할 수 있습니다. 따라서 융합 아키텍처는 기하학적 관계뿐만 아니라 이에 포함된 불확실성도 표현해야 합니다.

시간 정렬(Temporal Alignment) 역시 중요합니다. 센서 측정값은 서로 다른 물리적 시점을 나타낼 수 있기 때문입니다. 카메라, 라이다, 레이더, IMU, 엔코더는 서로 다른 갱신율(Update Rate)로 작동하며 서로 다른 지연시간(Latency)을 가질 수 있습니다. 타임스탬핑(Timestamping), 버퍼링(Buffering), 보간(Interpolation), 운동 보상(Motion Compensation), 상태 전파(State Propagation)를 사용하면 관측값을 공통 융합 시점(Common Fusion Time)에 맞출 수 있습니다. 공간적으로 정확한 측정값도 시간적 관계가 일치하지 않으면 잘못된 융합 상태를 생성할 수 있습니다.

공통 표현(Common Representation)은 다중모달 통합(Multimodal Integration)을 단순화할 수 있습니다. 조감도 표현(Bird\'s-Eye-View Representation)은 카메라와 거리 센서 정보를 공통의 지면 중심 공간으로 투영할 수 있으며, 복셀 그리드(Voxel Grid)와 점유 표현(Occupancy Representation)은 3차원 공간 구조를 제공합니다. 객체 중심 표현(Object-Centric Representation)은 탐지된 객체를 중심으로 정보를 구성하며, 잠재 표현(Latent Representation)은 신경망이 임무에 적합한 공유 공간을 구성하도록 합니다. 어떤 표현을 선택하는가에 따라 융합 시스템이 효율적으로 표현할 수 있는 관계가 달라집니다.

조감도 융합(Bird\'s-Eye-View Fusion)은 여러 카메라 시야, 라이다 기하 구조, 레이더 관측값, 지도(Map), 운동 정보를 로봇을 기준으로 하는 공통 공간 좌표계에 표현할 수 있기 때문에 이동형 피지컬 AI(Mobile Physical AI)에 특히 유용합니다. 이를 통해 객체 인지, 자유 공간 추정(Free-Space Estimation), 점유 추론(Occupancy Reasoning), 궤적 예측(Trajectory Prediction), 계획을 지원할 수 있습니다. 그러나 조감도로 투영하는 과정에서 수직 방향의 세부 정보가 손실될 수 있으므로 조작이나 복잡한 3차원 상호작용이 필요한 응용에서는 더욱 풍부한 표현이 필요할 수 있습니다.

점유 기반 융합(Occupancy-Based Fusion)은 모든 관측값을 객체 탐지 결과로 변환하도록 요구하지 않고 공간 영역이 비어 있는지, 점유되어 있는지 또는 불확실한지를 표현합니다. 이를 통해 여러 모달리티의 증거를 통합하면서 사전에 정의된 객체 범주에 속하지 않는 환경 구조도 유지할 수 있습니다. 피지컬 AI는 의미 인식(Semantic Recognition)이 완전하지 않은 상황에서도 이동 가능한 공간과 이동 불가능한 공간을 판단해야 하기 때문에 점유 표현은 내비게이션과 월드 모델링에서 특히 유용합니다.

불확실성은 융합 과정에서 제거해야 하는 요소가 아니라 융합된 표현의 일부로 취급해야 합니다. 센서 측정의 정확도는 거리, 날씨, 조명, 가림(Occlusion), 움직임, 운용 조건에 따라 달라집니다. 따라서 융합에서는 증거를 가중할 때 신뢰도(Confidence) 또는 불확실성을 고려해야 합니다. 정밀한 라이다 관측과 불확실한 카메라 추정값이 항상 동일한 비중으로 기여할 필요는 없으며, 환경 조건에 따라 이들의 상대적인 중요도가 동적으로 변화할 수 있습니다.

고전적 확률론적 방법(Classical Probabilistic Method)은 불확실성을 고려한 융합을 구현하는 하나의 접근법입니다. 칼만 필터(Kalman Filter)와 비선형 변형(Nonlinear Variant)은 추정된 상태 및 측정 불확실성에 따라 여러 측정값을 결합할 수 있으며, 파티클 필터(Particle Filter)는 더욱 복잡한 분포를 표현할 수 있습니다. 베이지안 추론(Bayesian Reasoning)은 새로운 증거를 이용하여 믿음(Belief)을 갱신하는 보다 일반적인 프레임워크를 제공합니다. 이러한 방법은 다른 영역에서 신경망 인지를 사용하더라도 위치 추정, 추적, 상태 추정, 안전 관련 기능에서 여전히 중요합니다.

딥러닝(Deep Learning)은 다중모달 데이터에서 직접 관계를 학습할 수 있는 학습 기반 융합(Learned Fusion) 메커니즘을 제공합니다. 연결(Concatenation), 어텐션(Attention), 교차 어텐션(Cross-Attention), 트랜스포머(Transformer), 게이팅 네트워크(Gating Network), 공유 잠재 공간(Shared Latent Space)을 사용하여 서로 다른 센서의 정보가 어떻게 상호작용해야 하는지를 결정할 수 있습니다. 학습 기반 융합은 수작업으로 정의하기 어려운 복잡한 의존 관계를 포착할 수 있지만 학습 데이터, 캘리브레이션 품질, 센서 가용성, 학습 과정에 포함된 환경 조건에 크게 의존합니다.

어텐션 기반 융합(Attention-Based Fusion)은 센서, 공간 영역 또는 시간적 관측값에 대한 계산적 중요도를 동적으로 할당할 수 있습니다. 하나의 모달리티가 유용한 정보를 충분히 제공하지 못하면 모델이 다른 정보 소스를 더욱 강조할 수 있습니다. 그러나 어텐션 가중치(Attention Weight) 자체가 물리적 신뢰성이나 고장 허용성(Fault Tolerance)을 보장하는 것은 아닙니다. 학습된 가중 방식은 센서 성능 저하, 입력 누락, 캘리브레이션 오류, 환경 변화, 학습 분포(Training Distribution)와 다른 조건에서 평가되어야 합니다.

융합 아키텍처에서는 누락되거나 성능이 저하된 모달리티(Missing or Degraded Modality)도 고려해야 합니다. 모든 센서가 정상적으로 작동하는 조건만으로 학습된 모델은 하나의 센서 스트림이 사라졌을 때 예상하지 못한 방식으로 실패할 수 있습니다. 모달리티 드롭아웃(Modality Dropout), 센서 마스킹(Sensor Masking), 불확실성 인식 학습(Uncertainty-Aware Training), 중복 표현(Redundant Representation), 명시적인 성능 저하 모드(Degraded Mode)는 회복탄력성(Resilience)을 향상시킬 수 있습니다. 시스템은 어떤 관측값을 사용할 수 있는지 인식해야 하며 누락된 정보를 유효한 0값의 물리적 측정으로 해석해서는 안 됩니다.

센서 융합과 월드 모델링은 점점 더 밀접하게 연결되고 있습니다. 각각의 인지 임무에 독립적인 출력을 생성하는 대신 다중모달 융합을 통해 기하 구조, 의미 정보, 움직임, 점유 상태, 불확실성, 시간 이력(Temporal History)을 포함하는 지속적인 월드 상태(Persistent World State)를 구성할 수 있습니다. 새로운 관측값은 이 상태를 갱신하고 예측 모델은 이후 상태가 어떻게 변화할지를 추정합니다. 따라서 융합은 동기화된 센서 프레임을 일회성으로 결합하는 과정이 아니라 지속적인 상태 추정(Continuous State Estimation)의 일부가 됩니다.

컴퓨팅 아키텍처에서 융합이 수행되는 위치도 중요합니다. 일부 처리는 스마트 센서(Smart Sensor) 내부, 분산 엣지 프로세서(Distributed Edge Processor), 중앙 GPU에서 수행할 수 있습니다. 분산 융합(Distributed Fusion)은 통신 요구량을 감소시키고 워크로드를 분리할 수 있는 반면 중앙 집중식 융합(Centralized Fusion)은 더욱 풍부한 교차 모달 정보에 접근할 수 있습니다. 최적의 분할은 대역폭, 지연시간, 동기화, 컴퓨팅 가용성, 전력, 열 한계(Thermal Limit), 신뢰성, 후속 모델이 요구하는 정보에 따라 결정됩니다.

아키텍처는 불필요하게 반복되는 변환과 데이터 이동을 피해야 합니다. 이미지, 포인트 클라우드, 복셀, 조감도, 객체 표현 사이를 반복적으로 투영하면 상당한 컴퓨팅과 메모리 대역폭을 소비하면서 보간 또는 양자화 오차(Quantization Error)를 발생시킬 수 있습니다. 신중하게 선택한 공유 표현(Shared Representation)은 이러한 변환을 감소시킬 수 있습니다. 따라서 센서 융합은 독립된 하나의 모듈로 삽입하기보다 전체 인지 및 월드 모델 파이프라인과 함께 설계해야 합니다.

평가(Evaluation)는 현실적인 다중모달 조건에서 후속 임무 성능(Downstream Task Performance)을 측정해야 합니다. 융합은 위치 추정, 추적, 점유 추정, 예측, 계획, 제어에도 영향을 미치므로 탐지 정확도만으로는 충분하지 않습니다. 시험에서는 조명, 날씨, 거리, 움직임, 가림, 센서 잡음, 동기화 오차, 캘리브레이션 드리프트(Calibration Drift), 센서 고장을 변화시켜야 합니다. 개별 모달리티가 모호해지는 조건에서 상호 보완적인 증거가 전체 시스템의 신뢰성을 향상시킬 때 융합의 가치가 입증됩니다.

융합 복잡도(Fusion Complexity)는 실시간 운용(Real-Time Operation)이 가능한 범위 안에 있어야 합니다. 정교한 다중모달 트랜스포머(Multimodal Transformer)는 표현 품질을 향상시킬 수 있지만 메모리 사용량, 추론 지연시간(Inference Latency), 대역폭, 전력 소비를 증가시킬 수 있습니다. 경우에 따라 더 단순한 확률론적 융합이나 특징 수준 융합만으로 충분한 성능과 더 높은 예측 가능성(Predictability)을 제공할 수 있습니다. 따라서 센서-AI 공동설계(Sensor-AI Co-Design)에서는 벤치마크 정확도만 최적화하지 않고 융합 품질을 계산 비용 및 센싱-행동 마감시간(Sensing-to-Action Deadline)과 함께 평가해야 합니다.

강건한 센서 융합 아키텍처(Robust Sensor Fusion Architecture)는 궁극적으로 시간적·공간적으로 일관된 표현을 제공하면서 불확실성을 후속 지능 시스템에서 확인할 수 있도록 유지해야 합니다. 모든 센서가 항상 정확하거나 사용 가능하다고 가정하지 않고 서로 보완적인 증거를 결합해야 합니다. 이렇게 생성된 상태를 통해 인지, 위치 추정, 예측, 계획, 제어 시스템은 현재 사용 가능한 최선의 정보를 기반으로 추론하면서도 지식이 불완전한 영역을 인식할 수 있습니다.

따라서 센서 융합 아키텍처(Sensor Fusion Architecture)는 시간 동기화 및 타임스탬핑(Time Synchronization and Timestamping)을 모델 주도 센서 선택(Model-Driven Sensor Selection), 센서 성능 저하 운용(Degraded-Sensor Operation), 정량적 센서-컴퓨팅-대역폭 예산화(Quantitative Sensor-Compute-Bandwidth Budgeting)와 연결합니다. 이는 서로 다른 물리적 측정값이 통합된 기계적 세계 표현(Unified Machine Representation of the World)으로 변환되는 지점입니다. 피지컬 AI에서 성공적인 융합은 가장 많은 센서를 결합하는 것으로 정의되는 것이 아니라 서로 보완적인 관측값을 지능적 행동(Intelligent Behavior)에 가장 신뢰할 수 있고 실행 가능한 상태(Actionable State)로 변환하는 것으로 정의됩니다.

## 04.10. AI Model Driven Sensor Selection

![](images/image12.png){width="7.268055555555556in" height="7.268055555555556in"}

AI 모델 주도 센서 선택(AI-Model-Driven Sensor Selection)은 센싱 하드웨어(Sensing Hardware)를 AI와 독립적으로 선택되는 고정된 장치 집합이 아니라 지능 아키텍처(Intelligence Architecture)의 일부로 다룹니다. 핵심 질문은 "어떤 센서가 가장 많은 데이터를 제공하는가?"에서 "모델이 임무 수행에 필요한 상태를 추정하려면 어떤 관측값이 필요한가?"로 변화합니다. 따라서 센서 선택은 모델 입력(Model Input), 표현(Representation), 불확실성 요구사항(Uncertainty Requirement), 후속 의사결정(Downstream Decision)에서 시작합니다.

피지컬 AI(Physical AI) 모델이 직접적으로 필요로 하는 것은 카메라(Camera), 라이다(LiDAR), 레이더(Radar), IMU와 같은 제품 범주 자체가 아닙니다. 모델이 필요로 하는 것은 기하 구조(Geometry), 의미 정보(Semantics), 움직임(Motion), 깊이(Depth), 속도(Velocity), 위치 추정(Localization), 지형(Terrain), 접촉(Contact), 로봇 내부 상태(Internal Robot State)에 대한 정보입니다. 서로 다른 센서 조합은 이러한 잠재적 물리 변수(Latent Physical Variable)를 서로 다른 정확도와 강건성(Robustness)으로 제공할 수 있습니다. 모델 주도 설계는 필요한 정보를 먼저 정의하고 이를 적절한 센싱 모달리티(Sensing Modality)로 역으로 연결하는 방식에서 시작합니다.

임무 아키텍처(Task Architecture)는 첫 번째 수준의 센서 요구사항을 정의합니다. 내비게이션(Navigation)은 자유 공간 기하 구조(Free-Space Geometry), 장애물 움직임, 위치 추정, 주행 가능성(Traversability)을 요구할 수 있으며, 조작(Manipulation)은 객체 자세(Object Pose), 표면 기하 구조, 접촉 상태, 관절 구성을 필요로 합니다. 고속 이동은 위험 요소를 더 일찍 관측하고 움직임을 파악해야 하는 반면 정밀 도킹(Precision Docking)은 정확한 근거리 기하 정보가 필요합니다. 따라서 센서는 성공적인 행동을 결정하는 상태를 기준으로 선택해야 합니다.

AI 표현(AI Representation)은 어떤 센서 정보가 가치 있는지를 더욱 변화시킵니다. 객체 중심 인지 시스템(Object-Centric Perception System)은 신뢰성 있는 탐지와 추적(Tracking)을 지원하는 측정값을 우선할 수 있는 반면, 점유 모델(Occupancy Model)은 자유 공간, 점유 공간, 미지 공간(Unknown Space)에 대한 조밀한 증거를 요구합니다. 조감도 모델(Bird\'s-Eye-View Model)은 공통 공간 좌표계로 일관되게 변환할 수 있는 센서에서 이점을 얻습니다. 월드 모델(World Model)은 추가적으로 동역학(Dynamics)과 미래 상태 전이(Future State Transition)를 추론할 수 있을 정도의 시간적 관측을 필요로 할 수 있습니다.

모델 아키텍처(Model Architecture)는 선호되는 모달리티 간 균형에도 영향을 줄 수 있습니다. 카메라 중심 모델(Camera-Dominant Model)은 풍부한 의미 정보와 성숙한 비전 백본(Vision Backbone)을 활용하고, 라이다 중심 시스템(LiDAR-Centric System)은 명시적인 기하 구조를 강조합니다. 레이더는 속도 추정과 악천후 강건성(Adverse-Weather Robustness)을 강화할 수 있으며, 고유수용감각(Proprioception)은 로봇 자체에 대한 필수 정보를 제공합니다. 다중모달 모델(Multimodal Model)은 상호 보완적인 증거를 활용할 수 있지만 모달리티가 추가될 때마다 인코딩(Encoding), 동기화(Synchronization), 캘리브레이션(Calibration), 융합(Fusion), 컴퓨팅 요구사항이 추가됩니다.

따라서 센서 선택에서는 센서 개수보다 정보 기여도(Information Contribution)를 고려해야 합니다. 추가 카메라가 중요한 사각 영역(Blind Zone)을 제거한다면 상당한 가치를 제공할 수 있지만 이미 충분히 관측되고 있는 영역을 중복해서 관측한다면 그 가치는 작을 수 있습니다. 마찬가지로 라이다를 추가하면 특정 모델에서는 기하학적 불확실성을 크게 줄일 수 있지만 다른 모델에서는 제한적인 성능 향상만 제공할 수 있습니다. 중요한 평가 기준은 추가적인 관측이 후속 AI 능력(Downstream AI Capability)에 제공하는 증분적 향상(Incremental Improvement)입니다.

유용한 개념 중 하나는 한계 정보 가치(Marginal Information Value)입니다. 후보 센서를 기존 센싱 구성에 추가했을 때 불확실성을 얼마나 감소시키거나 임무 성능을 얼마나 향상시키는지를 측정하여 평가할 수 있습니다. 특정 모달리티를 추가했을 때 탐지, 상태 추정(State Estimation), 예측(Prediction), 계획(Planning) 성능이 조금만 향상되는 반면 대역폭과 컴퓨팅 부하가 크게 증가한다면 시스템 비용을 정당화하기 어려울 수 있습니다. 따라서 센서 선택은 정보 이득(Information Gain)과 자원 사이의 최적화 문제가 됩니다.

절제 연구(Ablation Study)는 이러한 가치를 추정하기 위한 실용적인 방법을 제공합니다. 다중모달 모델에서 개별 센서를 제거하거나 성능을 저하시키고, 마스킹(Masking)하거나 낮은 충실도의 입력으로 교체한 후 성능을 평가할 수 있습니다. 이때 발생하는 인지 정확도, 위치 추정 오차(Localization Error), 추적 연속성(Tracking Continuity), 점유 품질(Occupancy Quality), 예측 성능, 계획 성공률의 변화는 모델이 각각의 정보 소스에 얼마나 의존하는지를 보여줍니다. 이러한 실험을 통해 겉보기에는 중복적으로 보이지만 특정 운용 조건에서 중요한 역할을 하는 센서를 식별할 수 있습니다.

센서의 중요도는 환경에 따라 일정하지 않습니다. 카메라는 양호한 조명 환경에서는 의미 인지(Semantic Perception)에 중요한 역할을 할 수 있지만 어둠, 눈부심(Glare), 안개, 오염 환경에서는 신뢰성이 감소할 수 있습니다. 라이다는 안정적인 기하 정보를 제공하지만 특정 날씨나 반사 조건의 영향을 받을 수 있습니다. 레이더는 의미적 세부 정보가 상대적으로 적지만 가시성이 좋지 않은 상황에서도 유용한 운동 정보를 유지할 수 있습니다. 따라서 모델 주도 선택에서는 평균적인 성능만이 아니라 목표 운용 범위(Operating Envelope) 전체에서 조건부 정보 가치(Conditional Information Value)를 평가해야 합니다.

불확실성 인식 모델(Uncertainty-Aware Model)은 이러한 관계를 명시적으로 표현할 수 있습니다. 모든 모달리티를 동일하게 신뢰하는 대신 모델이 신뢰도(Confidence)를 추정하여 각각의 관측값이 융합 상태(Fused State)에 얼마나 강하게 기여할지를 조정할 수 있습니다. 특정 환경에서 불확실성이 증가한 센서의 가중치를 낮추고 다른 모달리티의 영향력을 높일 수 있습니다. 이를 통해 모든 모달리티가 모든 환경에서 최상의 성능을 제공하도록 요구하는 대신 서로 보완적인 불확실성 특성(Complementary Uncertainty Profile)을 기준으로 센서를 선택할 수 있습니다.

중복성(Redundancy) 역시 모델의 관점에서 평가해야 합니다. 두 센서는 하나의 정보 소스를 사용할 수 없게 되었을 때 남아 있는 AI 기능이 핵심 상태를 계속 추정할 수 있는 경우에만 의미 있는 중복성을 제공합니다. 하드웨어 복제(Hardware Duplication)가 반드시 정보 중복성(Information Redundancy)을 보장하는 것은 아니며, 서로 다른 모달리티가 점진적 성능 저하(Graceful Degradation)에 충분한 중첩 정보를 제공할 수도 있습니다. 따라서 모델 주도 중복성 분석에서는 각각의 센서 손실 시나리오에서 어떤 임무 능력이 유지되는지를 평가합니다.

학습 전략(Training Strategy)은 모델이 선택된 관측값을 사용하는 방법을 학습해야 하기 때문에 센서 선택에 영향을 줍니다. 새로운 모달리티를 추가하려면 적절하게 동기화된 데이터셋(Dataset), 캘리브레이션, 전처리(Preprocessing), 데이터 증강(Augmentation), 모델 용량(Model Capacity)이 필요합니다. 특정 센서 조합에 대한 고품질 학습 데이터를 확보할 수 없다면 이론적인 정보적 장점이 실제 AI 성능으로 연결되지 않을 수 있습니다. 따라서 센서 가용성, 데이터셋 가용성, 학습 아키텍처(Learning Architecture)를 시스템 설계 과정에서 함께 고려해야 합니다.

모달리티 드롭아웃(Modality Dropout)은 센서가 고장나더라도 기능을 유지할 수 있는 모델을 학습하는 데 도움이 됩니다. 학습 과정에서 선택된 센서 입력을 의도적으로 제거하거나 성능을 저하시켜 모델이 대체 증거 경로(Alternative Evidence Path)를 학습하도록 합니다. 이것이 물리적 중복성의 필요성을 제거하지는 않지만 특정 모달리티에 대한 지나친 의존을 방지할 수 있습니다. 이러한 실험을 통해 허용 가능한 성능을 유지하는 센서 조합을 파악하여 하드웨어 단순화 또는 성능 저하 모드(Degraded Mode) 설계에 활용할 수도 있습니다.

AI 모델은 정적인 하드웨어 센서 선택뿐만 아니라 동적 센서 선택(Dynamic Sensor Selection)도 지원할 수 있습니다. 모든 센서나 처리 파이프라인을 항상 최대 충실도(Maximum Fidelity)로 작동시킬 필요는 없습니다. 단순하고 예측 가능한 이동 상황에서는 일부 센서의 프레임 레이트(Frame Rate), 해상도(Resolution), 추론 주파수(Inference Frequency)를 줄일 수 있습니다. 불확실성, 환경 복잡도, 속도, 상호작용, 위험이 증가하면 추가 센싱과 컴퓨팅을 활성화할 수 있습니다. 이 경우 센싱은 지능형 상태 추정(Intelligent State Estimation)에 의해 제어되는 적응형 자원(Adaptive Resource)이 됩니다.

이러한 접근법은 시스템이 불확실성을 감소시키거나 의사결정을 개선할 것으로 예상되는 관측을 선택하는 능동 인지(Active Perception)와 관련됩니다. 로봇은 행동하기 전에 카메라의 위치를 변경하거나 센서를 회전시키고, 관측점을 변경하거나 객체에 접근하고, 추가 측정을 요청할 수 있습니다. 고정된 데이터 스트림을 수동적으로 받아들이는 대신 AI가 다음에 무엇을 센싱해야 하는지 결정하는 과정에 참여합니다. 따라서 인지와 행동은 정보 탐색 행동(Information-Seeking Behavior)을 통해 서로 연결됩니다.

학습 기반 게이팅 메커니즘(Learned Gating Mechanism)은 또 다른 구현 전략을 제공합니다. 게이팅 네트워크(Gating Network)는 현재 상황에서 어떤 모달리티 또는 특징 스트림(Feature Stream)이 유용한지를 추정하여 선택적으로 활성화하거나 강조할 수 있습니다. 희소 혼합 메커니즘(Sparse Mixture Mechanism)과 조건부 컴퓨팅(Conditional Computation)도 매 주기마다 모든 센서를 모든 모델 분기를 통해 처리하는 것을 피할 수 있습니다. 이러한 기술은 평균적인 컴퓨팅 요구량을 줄일 수 있지만 최악 조건 지연시간(Worst-Case Latency)과 고장 동작은 여전히 실시간 요구사항을 만족해야 합니다.

센서 선택에서는 인지 지표(Perception Metric)만 최적화하지 않고 후속 계획과 제어까지 고려해야 합니다. 특정 센서는 객체 탐지 정확도를 약간만 향상시키더라도 충돌 경계(Collision Boundary) 주변의 불확실성을 크게 감소시켜 더욱 안전하고 부드러운 궤적을 생성할 수 있습니다. 반대로 벤치마크 인지 정확도의 향상이 실제 로봇 행동에는 거의 영향을 주지 않을 수도 있습니다. 따라서 올바른 목표는 독립적인 모델 정확도가 아니라 임무 수준 효용(Task-Level Utility)입니다.

월드 모델은 모델 주도 센싱(Model-Driven Sensing)을 적용하는 데 특히 중요한 맥락을 제공합니다. 월드 모델은 환경의 잠재 상태(Latent State)를 지속적으로 유지하고 예측하려 하기 때문에 관측값이 상태 추정과 미래 예측을 얼마나 개선하는지를 기준으로 센서 가치를 평가할 수 있습니다. 기하 구조, 의미 정보, 움직임, 상호작용, 로봇 상태에 대해 상호 보완적인 정보를 제공하는 센서는 학습된 세계 표현(Learned World Representation)의 서로 다른 차원에서 불확실성을 감소시킬 수 있습니다.

시간적 예측(Temporal Prediction)은 센싱 요구량을 감소시킬 수 있는 기회도 제공합니다. 월드 모델이 천천히 변화하는 상태를 높은 신뢰도로 예측할 수 있다면 반복적인 고비용 관측은 제한적인 새로운 정보만 제공할 수 있습니다. 예측 불확실성(Prediction Uncertainty)이 증가하거나 예상하지 못한 사건이 발생하면 센싱 강도를 높일 수 있습니다. 이러한 예측 오차 주도 전략(Prediction-Error-Driven Strategy)을 통해 시스템은 현재 물리 세계가 얼마나 예상 밖이거나 불확실한지에 따라 센서와 컴퓨팅 자원을 할당할 수 있습니다.

자원 제약(Resource Constraint)은 센서 선택 목표의 일부로 계속 포함되어야 합니다. 각각의 센서는 전력, 물리적 공간, 질량, 통신 대역폭(Communication Bandwidth), 메모리 대역폭(Memory Bandwidth), 컴퓨팅 용량, 열 예산(Thermal Budget), 엔지니어링 노력을 소비합니다. 센서 데이터는 또한 캘리브레이션, 동기화, 전처리, 저장(Storage), 유지보수 요구사항을 발생시킵니다. 따라서 센서 구성은 센서가 제공하는 정보가 전체 수명주기 비용(Lifecycle Cost)과 컴퓨팅 비용을 정당화할 때 바람직하다고 할 수 있습니다.

최적화(Optimization)는 임무 성능, 불확실성, 안전(Safety), 강건성, 지연시간(Latency), 전력, 대역폭, 컴퓨팅, 비용에 대한 목적함수를 정의하여 이러한 절충 관계를 공식화할 수 있습니다. 이후 시뮬레이션(Simulation), 기록된 데이터셋, 하드웨어 인 더 루프 시험(Hardware-in-the-Loop Testing), 실제 환경 실험을 통해 후보 센서 구성을 비교할 수 있습니다. 목표는 반드시 수학적으로 완벽한 최적해를 찾는 것이 아니라 센싱 자원이 측정 가능한 AI 요구사항과 직접 연결되는 합리적이고 설명 가능한 아키텍처를 구축하는 것입니다.

모델 주도 센서 선택에서는 미래 확장성(Future Extensibility)도 고려해야 합니다. 현재의 특정 모델에 지나치게 최적화된 구성은 AI 아키텍처가 발전할 때 제약이 될 수 있습니다. 전략적으로 가치 있는 원시 정보(Raw Information), 표준화된 인터페이스(Standardized Interface), 캘리브레이션 기능, 여유 대역폭(Spare Bandwidth)을 유지하면 미래 모델을 지원할 수 있습니다. 따라서 공동설계(Co-Design)는 현재의 효율성과 충분한 아키텍처 유연성(Architectural Flexibility) 사이에서 균형을 유지하여 센싱 하드웨어가 소프트웨어 발전을 지나치게 일찍 제한하지 않도록 해야 합니다.

검증(Validation)은 정상 운용뿐만 아니라 환경적 성능 저하(Environmental Degradation), 센서 고장, 컴퓨팅 과부하(Computational Overload), 분포 변화(Distribution Shift)를 포함해야 합니다. 각각의 조건은 선택된 센서가 모델이 요구하는 기능을 유지하는 데 충분한 정보를 제공하는지를 보여줍니다. 성능은 인지 정확도뿐만 아니라 불확실성, 예측 품질, 계획 성공률, 제어 안정성(Control Stability), 안전 여유(Safety Margin), 제한된 기능의 운용 상태로 점진적으로 전환할 수 있는 능력을 기준으로 평가해야 합니다.

결과적으로 만들어지는 아키텍처는 센서, AI 표현, 융합, 컴퓨팅, 행동을 하나의 결합된 설계 공간(Coupled Design Space)으로 다룹니다. 센서 선택은 더 이상 AI 개발이 시작되기 전에 완료되는 독립적인 단계가 아니며, 센싱 구성과 모델을 반복적으로 함께 평가하는 과정이 됩니다. 모델은 어떤 관측값이 유용한 정보를 제공하는지를 보여주고, 하드웨어 제약은 로봇의 물리적·컴퓨팅 자원 범위 안에서 어떤 관측값을 신뢰성 있게 획득할 수 있는지를 결정합니다.

따라서 AI 모델 주도 센서 선택(AI-Model-Driven Sensor Selection)은 센서 융합(Sensor Fusion)의 질문을 사용 가능한 측정값을 어떻게 결합할 것인가에서 애초에 어떤 측정값이 존재해야 하는가라는 더욱 근본적인 질문으로 확장합니다. 목표는 최대한 많은 센싱(Maximum Sensing)이 아니라 신뢰성 있는 지능에 필요한 충분하고 상호 보완적인 정보(Sufficient and Complementary Information)를 확보하는 것입니다. 잘 설계된 피지컬 AI 시스템은 실제 환경의 제약 속에서 강건한 행동(Robust Action)을 지원할 수 있도록 모델이 필요로 하는 정보를 필요한 위치와 시점에서 충분한 중복성과 충실도(Fidelity)를 가지고 센싱합니다.

## 04.11. Degraded Sensor and Fallback Operation

![](images/image13.png){width="7.268055555555556in" height="7.268055555555556in"}

센서 성능 저하 운용(Degraded-Sensor Operation)은 센싱 능력이 부분적으로 사용할 수 없거나 신뢰성이 저하되거나 불확실해졌을 때에도 피지컬 AI(Physical AI) 시스템이 안전하게 기능을 지속할 수 있는 능력을 의미합니다. 실제 로봇은 모든 카메라(Camera), 라이다(LiDAR), 레이더(Radar), IMU, 엔코더(Encoder), 고유수용감각 센서(Proprioceptive Sensor)가 항상 정상적인 정보를 제공한다고 가정할 수 없습니다. 따라서 강건한 아키텍처(Robust Architecture)는 센서 성능 저하를 예외적인 소프트웨어 고장이 아니라 예상 가능한 운용 조건으로 다루어야 합니다.

센서 성능 저하(Sensor Degradation)는 완전한 하드웨어 고장만을 의미하지 않습니다. 카메라는 계속 작동하면서도 어둠, 눈부심(Glare), 오염, 모션 블러(Motion Blur), 부분적인 가림(Occlusion)으로 성능이 저하될 수 있습니다. 라이다는 비, 먼지, 반사 표면 또는 광학 경로 차단으로 성능이 악화될 수 있습니다. 레이더는 간섭(Interference)이나 다중경로 효과(Multipath Effect)를 겪을 수 있으며, IMU와 엔코더에는 바이어스(Bias), 드리프트(Drift), 잡음, 포화(Saturation), 통신 문제가 발생할 수 있습니다. 시스템은 이러한 상태를 정상 센싱과 구별할 수 있어야 합니다.

첫 번째 요구사항은 센서 상태 인식(Sensor-Health Awareness)입니다. 피지컬 AI는 입력되는 관측값이 계속해서 타당하고, 적시에 도착하며, 일관되고, 충분한 정보를 제공하는지를 지속적으로 평가해야 합니다. 상태 지표에는 누락된 프레임, 비정상적인 갱신율(Update Rate), 타임스탬프(Timestamp) 오류, 신호 강도, 잡음 통계, 온도, 캘리브레이션 잔차(Calibration Residual), 통신 상태, 다른 센서와의 불일치 등이 포함될 수 있습니다. 센서가 계속 데이터를 출력한다고 해서 자동으로 신뢰할 수 있는 센서로 판단해서는 안 됩니다.

센서 간 일관성(Cross-Sensor Consistency)은 또 다른 중요한 진단 메커니즘을 제공합니다. 카메라, 라이다, 레이더, 오도메트리(Odometry), 관성 추정(Inertial Estimation)이 일반적으로 움직임이나 환경 구조에 대해 일치하는데 지속적인 불일치가 발생한다면 센서 성능 저하를 의미할 수 있습니다. 시스템은 예측된 측정값과 실제 관측값을 비교하고 시간에 따른 잔차(Residual)를 모니터링할 수 있습니다. 이러한 일관성 검사는 센서 융합(Sensor Fusion)을 단순한 수동적 결합 과정에서 비정상적인 센싱 동작을 탐지하는 능동적 메커니즘으로 변화시킵니다.

센서 상태 추정(Sensor-Health Estimation)은 단순히 정상 또는 고장이라는 이진 상태만이 아니라 불확실성(Uncertainty)을 표현해야 합니다. 실제 센서의 성능 저하는 점진적으로 진행되는 경우가 많으며 측정값이 부분적으로는 계속 유용할 수 있습니다. 신뢰도 점수(Confidence Score), 공분산 추정(Covariance Estimate), 품질 지표(Quality Indicator), 학습 기반 신뢰성 추정(Learned Reliability Estimate)을 사용하여 변화하는 센서 품질을 표현할 수 있습니다. 그러면 융합 및 계획 시스템은 여전히 유용한 정보를 제공하는 센서를 갑자기 완전히 제외하는 대신 불확실한 정보에 대한 의존도를 점진적으로 낮출 수 있습니다.

대체 운용(Fallback Operation)은 센서 성능이 저하된 이후에도 어떤 능력을 계속 관측할 수 있는지를 식별하는 것에서 시작합니다. 하나의 카메라가 고장나더라도 중첩된 카메라가 충분한 환경 커버리지를 유지할 수 있습니다. 라이다의 신뢰성이 저하되더라도 카메라와 레이더가 더 높은 기하학적 불확실성을 감수하면서 장애물 인지를 지원할 수 있습니다. GNSS를 사용할 수 없게 되면 비전, 라이다, 관성 및 휠 오도메트리(Wheel Odometry)를 통해 일정 시간 동안 위치 추정을 유지할 수 있습니다. 중요한 질문은 어떤 장치가 고장났는지가 아니라 어떤 임무 관련 상태(Task-Relevant State)를 여전히 신뢰성 있게 추정할 수 있는가입니다.

이는 자연스럽게 능력 기반 성능 저하(Capability-Based Degradation)로 이어집니다. 로봇은 자신의 상태를 단순히 작동 또는 고장으로 분류할 필요가 없습니다. 대신 위치 추정, 장애물 탐지, 자유 공간 추정(Free-Space Estimation), 속도 추정, 도킹(Docking), 조작(Manipulation), 장거리 인지(Long-Range Perception)와 같이 현재 사용할 수 있는 능력 집합을 유지할 수 있습니다. 센서 상태 정보는 이러한 능력 상태(Capability State)를 지속적으로 갱신하며 시스템이 충분한 신뢰도로 실제 인지할 수 있는 범위에 맞추어 행동하도록 합니다.

대체 모드(Fallback Mode)는 고장이 발생한 이후 즉흥적으로 결정하기보다 배치 전에 설계해야 합니다. 주요 센서 손실 또는 성능 저하 시나리오마다 대체 센싱 경로(Alternative Sensing Path), 축소된 운용 범위(Reduced Operating Envelope), 허용되는 행동을 정의할 수 있습니다. 예를 들어 장거리 인지 기능을 잃으면 속도를 줄여야 할 수 있으며, 정밀 깊이 정보를 사용할 수 없게 되면 도킹이나 조작 기능을 비활성화해야 할 수 있습니다. 대체 정책(Fallback Policy)은 센싱 능력을 안전한 운용 한계와 직접 연결해야 합니다.

충분한 정보가 남아 있다면 갑작스럽게 자율 기능을 상실하는 것보다 점진적 성능 저하(Graceful Degradation)가 바람직합니다. 시스템은 속도를 낮추고, 추종 거리(Following Distance)를 늘리고, 복잡한 지형을 피하며, 회전율을 제한하고, 장애물 안전 여유를 확대하거나, 추월을 중단하거나, 안전한 위치로 이동할 수 있습니다. 이러한 행동은 남아 있는 센서가 처리해야 하는 정보 및 반응 요구량을 감소시킵니다. 따라서 물리적 행동(Physical Behavior) 자체가 센싱 대체 전략의 일부가 됩니다.

중복성(Redundancy)은 대체 정보 경로를 제공할 때 가치가 있습니다. 동일한 센서 두 개는 하드웨어 고장에 대비할 수 있지만 이종 중복성(Heterogeneous Redundancy)은 환경적인 고장 모드로부터 시스템을 보호할 수 있습니다. 카메라와 라이다는 조명과 기하학적 조건에 서로 다르게 반응하며, 레이더는 광학 센싱의 성능이 저하되는 환경에서도 유용한 운동 정보를 유지할 수 있습니다. 따라서 효과적인 중복성은 단순한 센서 개수가 아니라 고장 독립성(Failure Independence)과 보존되는 AI 능력을 기준으로 평가해야 합니다.

입력 품질이 변화하면 센서 융합도 적응해야 합니다. 성능이 저하된 모달리티(Modality)가 계속 잘못된 정보를 제공하는 상황에서 고정된 융합 가중치(Fixed Fusion Weight)는 위험할 수 있습니다. 불확실성 인식 융합(Uncertainty-Aware Fusion)은 신뢰성이 낮은 관측값의 영향을 줄일 수 있으며, 게이팅 메커니즘(Gating Mechanism)은 손상된 특징을 억제하거나 더 정상적인 모달리티로 어텐션(Attention)을 전환할 수 있습니다. 중요한 정보 소스를 잃었을 때 융합된 표현(Fused Representation)은 증가한 불확실성을 명시적으로 반영해야 합니다.

AI 모델 역시 불완전한 센싱(Incomplete Sensing) 조건을 고려하여 학습되어야 합니다. 완벽한 센서 조합만을 사용하여 학습한 다중모달 모델(Multimodal Model)은 특정 모달리티가 사라질 때 쉽게 붕괴하는 의존 관계를 학습할 수 있습니다. 모달리티 드롭아웃(Modality Dropout), 센서 마스킹(Sensor Masking), 합성 손상(Synthetic Corruption), 잡음 주입(Noise Injection), 캘리브레이션 교란(Calibration Perturbation), 부분 가림을 이용하여 모델을 성능 저하 조건에 노출할 수 있습니다. 학습은 센서를 사용하는 방법뿐만 아니라 일부 정보가 없을 때 운용하는 방법도 시스템이 익히도록 해야 합니다.

대체 아키텍처(Fallback Architecture)에는 전체 AI 인지 스택에 의존하지 않는 독립적인 안전 채널(Independent Safety Channel)을 포함할 수 있습니다. 단순한 장애물 탐지기, 근접 센서(Proximity Sensor), 비상 제동 기능(Emergency Braking Function), 저수준 운동 모니터(Low-Level Motion Monitor)는 메인 다중모달 모델의 신뢰성이 떨어진 경우에도 작동할 수 있습니다. 이러한 채널은 주 시스템보다 지능 수준은 낮더라도 로봇이 감속하거나 정지하거나 다른 운용 모드로 전환하는 동안 필수적인 안전 기능을 유지할 수 있습니다.

위치 추정(Localization)의 성능 저하는 전체 자율주행 스택으로 전파될 수 있으므로 특별한 주의가 필요합니다. 위치 추정 불확실성이 증가하면 지도(Map), 탐지 객체, 예측 궤적(Predicted Trajectory), 계획 경로(Planned Path)가 모두 공간적으로 불일치할 수 있습니다. 따라서 대체 시스템은 위치 추정 신뢰도를 모니터링하고 다른 오도메트리 정보원이 운용을 유지할 수 있는지를 판단해야 합니다. 불확실성이 검증된 한계(Validated Limit)를 초과하면 로봇은 움직임을 줄이거나 안전 정지(Safe Stop) 상태로 전환해야 할 수 있습니다.

시간적 고장(Temporal Failure)은 공간적 센싱 고장만큼 위험할 수 있습니다. 센서가 정확한 측정값을 생성하더라도 지나치게 늦게 도착하거나 잘못된 타임스탬프를 포함하거나 간헐적으로 사용할 수 없게 될 수 있습니다. 이러한 데이터는 센싱 하드웨어 자체가 정상적으로 작동하더라도 융합 결과를 손상시킬 수 있습니다. 따라서 센서 상태 모니터링은 신호 품질뿐만 아니라 측정 데이터 연령(Measurement Age), 동기화 품질(Synchronization Quality), 통신 지연, 패킷 손실(Packet Loss), 갱신 규칙성(Update Regularity)을 포함해야 합니다.

캘리브레이션 성능 저하(Calibration Degradation)는 또 다른 미묘한 고장 모드입니다. 기계적 진동, 충격, 열팽창(Thermal Expansion), 유지보수, 센서 교체로 인해 센서의 방향이나 위치가 변화할 수 있습니다. 각각의 측정값은 정상적으로 보이면서도 교차 모달 투영(Cross-Modal Projection)은 잘못될 수 있습니다. 온라인 일관성 검사를 통해 증가하는 캘리브레이션 잔차를 탐지할 수 있으며 일부 시스템은 운용 중 제한적인 캘리브레이션 보정을 추정할 수도 있습니다. 불확실성이 심각한 경우 영향을 받은 융합 관계에 대한 의존도를 낮춰야 합니다.

컴퓨팅 과부하(Compute Overload) 역시 가상 센서 성능 저하(Virtual Sensor Degradation)를 발생시킬 수 있습니다. AI 컴퓨터가 입력 데이터를 요구되는 마감시간 안에 처리하지 못하면 프레임이 누락되거나 관측값이 오래된 상태가 될 수 있습니다. 의사결정 시스템의 관점에서는 이것도 센싱 능력을 잃는 것과 유사합니다. 대체 로직(Fallback Logic)은 이미지 해상도를 낮추고 처리 주기를 줄이며, 중요하지 않은 모델을 비활성화하고, 안전 관련 파이프라인을 우선 처리하거나 충분한 컴퓨팅 여유가 복구될 때까지 로봇 속도를 줄일 수 있습니다.

대체 운용에서는 성능 저하의 지속 시간(Duration of Degradation)도 고려해야 합니다. 짧은 카메라 데이터 손실은 시간적 추적(Temporal Tracking)이나 월드 모델 예측(World-Model Prediction)을 이용하여 보완할 수 있지만 장시간의 센서 손실에서는 예측 불확실성이 증가합니다. 상태 추정기는 이전 정보를 일정 시간 동안 전파할 수 있지만 예측된 상태를 새로운 관측값과 동일하게 취급해서는 안 됩니다. 시스템은 직접적인 센싱을 사용할 수 없을 때 신뢰도가 얼마나 빠르게 감소하는지를 이해해야 합니다.

월드 모델(World Model)은 신뢰할 수 있는 관측값 사이에서 환경 상태를 예측함으로써 일시적인 회복탄력성(Temporary Resilience)을 지원할 수 있습니다. 센서를 잠시 사용할 수 없게 되면 시간적 동역학(Temporal Dynamics)을 이용하여 객체, 점유 상태, 로봇 움직임에 대한 추정값을 유지할 수 있습니다. 그러나 센서가 사용할 수 없는 동안 예상하지 못한 사건을 관측할 수 없기 때문에 예측에는 증가하는 불확실성이 함께 반영되어야 합니다. 따라서 대체 설계에서는 예측을 물리적 센싱의 무제한 대체물이 아니라 일시적인 연결 수단(Bridge)으로 사용해야 합니다.

운용 설계 영역(Operational Design Domain)은 성능 저하 조건을 명시적으로 포함해야 합니다. 검증에서는 센서 가용성, 환경 조건, 속도, 지형, 컴퓨팅 상태의 어떤 조합에서 각각의 자율 기능을 허용할 수 있는지를 정의해야 합니다. 이에 따라 생성되는 성능 저하 운용 범위(Degraded Operating Envelope)는 정상 운용 범위보다 좁을 수 있습니다. 이를 통해 센서 상태와 허용 가능한 로봇 행동 사이에 체계적인 관계를 구축하고 고장 상황에서 비공식적인 판단에 의존하는 것을 방지할 수 있습니다.

시험(Testing)에서는 현실적인 고장을 의도적으로 주입해야 합니다. 센서를 분리하거나 지연시키고, 가리거나, 잘못 캘리브레이션하고, 포화시키거나, 잡음을 추가하고, 낮은 프레임 레이트로 작동시키거나, 시뮬레이션된 환경 간섭에 노출할 수 있습니다. 평가에서는 탐지, 위치 추정, 추적, 예측, 계획, 제어 안정성, 모드 전환 동작(Transition Behavior), 정지 안전성(Stopping Safety)을 측정해야 합니다. 고장 주입(Failure Injection)은 정상적인 시험에서는 발견되지 않을 수 있는 숨겨진 의존성을 드러냅니다.

운용 모드 사이의 전환(Transition)도 신중하게 설계해야 합니다. 센서 품질이 임계값 근처에서 변동할 때 정상 상태와 성능 저하 상태 사이를 빠르게 전환하면 불안정한 행동이 발생할 수 있습니다. 히스테리시스(Hysteresis), 지속성 검사(Persistence Check), 신뢰도 필터링(Confidence Filtering), 상태 머신(State Machine)을 사용하면 불필요한 모드 진동(Mode Oscillation)을 방지할 수 있습니다. 복구 과정에서도 단 한 번의 정상 측정값만으로 즉시 최고 속도 운용으로 복귀하기보다 센싱이 안정적인 상태로 회복되었다는 충분한 증거를 요구해야 합니다.

통신 아키텍처와 응용 환경이 허용한다면 사람의 감독(Human Supervision)이나 원격 지원(Remote Assistance)이 또 다른 대체 계층으로 활용될 수 있습니다. 로봇은 성능이 저하된 기능, 불확실성, 고장난 센서, 현재의 안전 상태를 운영자에게 보고할 수 있습니다. 그러나 통신 자체가 지연되거나 사용할 수 없을 수 있으므로 즉각적인 충돌 위험을 원격 개입이 해결해 줄 것이라고 가정해서는 안 됩니다. 로컬 안전 동작(Local Safety Behavior)은 외부 지원과 독립적으로 시스템을 보호할 수 있어야 합니다.

유용한 대체 계층(Fallback Hierarchy)은 적응형 융합(Adaptive Fusion)에서 시작하여 제한된 기능을 거쳐 최종적으로 안전 상태(Safe State)로 진행합니다. 초기에는 정상적인 모달리티의 가중치를 높여 시스템이 보상할 수 있습니다. 불확실성이 계속 증가하면 속도, 임무 또는 운용 영역을 제한할 수 있습니다. 핵심 상태를 충분한 신뢰도로 더 이상 관측할 수 없다면 자율 이동을 제한하거나 정지해야 합니다. 전환 기준은 임의적인 센서 개수가 아니라 측정 가능한 능력(Measurable Capability)을 기준으로 결정해야 합니다.

목표는 가능한 모든 고장 상황에서도 로봇이 원래의 임무를 계속 수행하도록 만드는 것이 아닙니다. 불충분한 센싱 상태에서도 완전한 기능을 유지하려 하면 위험한 과신(Unsafe Confidence)을 발생시킬 수 있습니다. 목표는 현재 사용 가능한 정보가 정당화할 수 있는 최대 수준의 유용한 행동을 유지하는 동시에 시스템이 자신의 지식이 의도된 행동을 수행하기에 더 이상 충분하지 않은 시점을 인식하도록 하는 것입니다.

따라서 센서 성능 저하 및 대체 운용(Degraded-Sensor and Fallback Operation)은 센서 선택(Sensor Selection), 중복성, 융합, 불확실성, 컴퓨팅 자원, 로봇 행동 사이의 연결을 완성합니다. 성숙한 피지컬 AI 시스템은 모든 하드웨어가 정상적으로 작동할 때만 주변을 인지하는 것이 아니라 자신의 인지 품질(Perception Quality)을 지속적으로 이해합니다. 강건한 자율성(Robust Autonomy)은 시스템이 어떤 정보를 잃었는지를 탐지하고, 무엇을 여전히 관측할 수 있는지를 추정하며, 이에 따라 지능과 움직임을 적응시키고, 신뢰성 있는 인지를 더 이상 유지할 수 없을 때 안전 상태로 전환할 수 있을 때 구현됩니다.

## 04.12. Sensor Compute Bandwidth Budgeting [w/Code]

![](images/image14.png){width="7.268055555555556in" height="7.268055555555556in"}

센서-컴퓨팅-대역폭 예산화(Sensor-Compute-Bandwidth Budgeting)는 센싱 요구사항을 전체 피지컬 AI(Physical AI) 파이프라인의 명시적인 자원 한계(Resource Limit)로 변환하는 과정입니다. 모든 카메라(Camera), 라이다(LiDAR), 레이더(Radar), IMU, 엔코더(Encoder), 고유수용감각 센서(Proprioceptive Sensor)는 통신, 메모리, 연산, 전력, 열 용량을 소비합니다. 따라서 실현 가능한 아키텍처는 필요한 모든 관측값을 로봇의 실시간 자원 범위(Real-Time Resource Envelope) 안에서 획득하고, 전송하고, 처리하고, 융합하며, 최종 행동으로 연결할 수 있는지를 검증해야 합니다.

예산화 과정(Budgeting Process)은 하드웨어 사양이 아니라 임무 요구사항(Task Requirement)에서 시작해야 합니다. 필요한 탐지 거리(Detection Distance), 위치 추정 정확도(Localization Accuracy), 장애물 크기, 로봇 속도, 제어 주파수(Control Frequency), 예측 지평(Prediction Horizon), 안전 여유(Safety Margin)가 필요한 센싱 충실도(Sensing Fidelity)를 결정합니다. 이후 이러한 요구사항을 센서 해상도(Resolution), 시야각(Field of View), 거리(Range), 갱신율(Update Rate), 중복성(Redundancy), 동기화(Synchronization) 목표로 변환한 다음 그에 따른 대역폭과 연산 요구량을 계산할 수 있습니다.

각각의 센서는 명시적인 데이터 전송률 모델(Data-Rate Model)을 가져야 합니다. 비압축 카메라(Uncompressed Camera)의 대략적인 대역폭은 이미지 폭, 이미지 높이, 픽셀당 비트 수(Bits per Pixel), 초당 프레임 수(Frames per Second)를 이용하여 추정할 수 있습니다. 라이다 대역폭은 초당 포인트 수(Points per Second)와 각 포인트에 저장되는 바이트 수에 따라 달라지며, 레이더는 탐지 결과, 포인트 클라우드(Point Cloud), 조밀한 텐서(Dense Tensor) 중 무엇을 출력하는지에 따라 크게 달라집니다. IMU와 엔코더는 상대적으로 작은 데이터 스트림을 생성하지만 높은 주파수와 결정론적인 전송(Deterministic Delivery)이 요구될 수 있습니다.

명목 센서 데이터 전송률(Nominal Sensor Data Rate)은 대역폭 예산의 첫 번째 계층에 불과합니다. 프로토콜 헤더(Protocol Header), 패킷화(Packetization), 동기화 정보, 재전송(Retransmission), 메타데이터(Metadata), 네트워크 관리, 안전 여유에도 추가적인 용량이 사용됩니다. 또한 여러 센서가 트래픽을 시간적으로 균등하게 분산시키지 않고 동시에 버스트(Burst)를 발생시킬 수 있습니다. 따라서 통신 링크는 이론적인 인터페이스 대역폭을 항상 완전히 사용할 수 있다고 가정하지 않고 현실적인 최대 사용률(Peak Utilization)과 지연시간(Latency) 특성을 기준으로 설계해야 합니다.

물리적 데이터 경로(Physical Data Path)는 종단 간(End-to-End)으로 예산화해야 합니다. 센서 정보는 최종 모델에 도달하기 전에 이더넷 스위치(Ethernet Switch), USB 컨트롤러(USB Controller), MIPI 인터페이스, PCIe 링크, CPU 메모리, GPU 메모리, 저장 장치, 프로세서 간 통신(Interprocessor Communication)을 통과할 수 있습니다. 높은 용량의 센서 인터페이스만으로 데이터 경로의 다른 병목현상(Bottleneck)을 해결할 수는 없습니다. 실질적인 처리량(Effective Throughput)은 센싱에서 컴퓨팅까지의 경로에서 가장 취약한 자원에 의해 제한됩니다.

메모리 대역폭(Memory Bandwidth)은 흔히 숨겨진 제약조건이 됩니다. 센서 텐서(Sensor Tensor)는 CPU와 가속기 메모리 사이에서 여러 차례 복사, 디코딩(Decoding), 크기 조정(Resizing), 투영(Projection), 정규화(Normalization), 복셀화(Voxelization), 융합 및 전송될 수 있습니다. 중간 신경망 표현(Intermediate Neural Representation)은 원래의 측정 데이터보다 훨씬 커질 수도 있습니다. 따라서 예산화에서는 입력 데이터 크기뿐만 아니라 전처리(Preprocessing), 신경망 추론(Neural Inference), 시간적 버퍼링(Temporal Buffering), 다중모달 융합(Multimodal Fusion)에서 발생하는 내부 메모리 트래픽도 추정해야 합니다.

컴퓨팅 예산화(Compute Budgeting)는 AI 스택(AI Stack)을 측정 가능한 워크로드(Workload)로 분해해야 합니다. 이미지 전처리, 포인트 클라우드 필터링(Point-Cloud Filtering), 레이더 처리, 위치 추정(Localization), 특징 추출(Feature Extraction), 다중모달 융합, 탐지(Detection), 점유 추정(Occupancy Estimation), 추적(Tracking), 월드 모델링(World Modeling), 예측(Prediction), 계획(Planning), 제어(Control)는 모두 자원을 소비합니다. 주요 인지 네트워크(Perception Network)의 연산량만 추정하면 메인 AI 모델과 동시에 실행되는 수많은 보조 알고리즘 때문에 전체 시스템 요구량을 크게 과소평가할 수 있습니다.

평균 컴퓨팅 사용률(Average Compute Utilization)만으로는 실시간 피지컬 AI를 평가하기에 충분하지 않습니다. 복잡한 장면에서는 객체 수, 추적 연관(Tracking Association), 지도 갱신(Map Update), 계획 복잡도, 신경망 워크로드가 증가할 수 있습니다. 여러 센서가 거의 동시에 처리를 요구할 수도 있습니다. 따라서 자원 예산에는 최대 또는 높은 백분위수 워크로드(Peak or High-Percentile Workload), 실행시간 변동(Execution-Time Variance), 일시적인 부하 증가가 큐(Queue) 누적이나 마감시간 위반으로 이어지지 않도록 충분한 컴퓨팅 여유를 포함해야 합니다.

지연시간 예산화(Latency Budgeting)는 자원 소비를 물리적 안전과 연결합니다. 전체 센싱-행동 지연시간(Sensing-to-Action Latency)은 센서 노출 또는 스캐닝, 데이터 전송, 버퍼링(Buffering), 전처리, 추론(Inference), 융합(Fusion), 예측, 계획, 명령 생성(Command Generation), 액추에이터 응답(Actuator Response)을 포함합니다. 각각의 구성 요소에는 개별 지연시간 예산을 할당해야 하며, 그 총합은 로봇 속도, 정지 거리(Stopping Distance), 장애물 기하 구조, 필요한 안전 여유에 의해 결정되는 최대 반응시간(Maximum Reaction Time)보다 작아야 합니다.

큐잉(Queueing)은 반드시 고려해야 합니다. 과부하된 파이프라인은 계산 자체는 계속 수행하면서도 시간적으로는 무의미한 상태가 될 수 있기 때문입니다. 데이터가 들어오는 속도보다 처리 속도가 느리면 측정값이 누적되고 AI는 점점 더 오래된 세계를 기준으로 추론하게 됩니다. 따라서 GPU 사용률만 모니터링하면 위험한 상태를 놓칠 수 있습니다. 데이터 연령(Data Age), 큐 깊이(Queue Depth), 누락된 프레임(Dropped Frame), 처리 마감시간(Processing Deadline), 종단 간 지연시간을 운용 자원 예산에 직접 포함해야 합니다.

센서 해상도와 갱신율은 여러 자원에 동시에 영향을 주기 때문에 강력한 예산 조정 변수(Budget Variable)입니다. 카메라 해상도를 높이면 통신 트래픽, 메모리 이동, 전처리 비용, 신경망 연산량이 함께 증가합니다. 라이다 밀도(LiDAR Density)를 높이면 포인트 처리량과 기하 처리 비용이 증가합니다. 높은 갱신율은 전체 파이프라인의 실행 빈도를 높입니다. 따라서 이러한 매개변수는 각각 최대의 센싱 품질을 목표로 독립적으로 최적화하기보다 공동으로 조정해야 합니다.

센서 부하(Sensor Load)와 컴퓨팅 부하(Compute Load)의 관계가 항상 선형적인 것은 아닙니다. 이미지 픽셀 수를 두 배로 증가시키면 여러 신경망 계층에서 특징 맵(Feature Map)의 크기가 변화할 수 있으며, 카메라를 추가하면 다중 시점 투영(Cross-View Projection)과 어텐션(Attention) 연산이 추가될 수 있습니다. 새로운 모달리티(Modality)를 추가하면 새로운 인코더(Encoder)가 필요하고 융합 복잡도가 증가할 수 있습니다. 따라서 시스템 요구량은 원시 센서 대역폭만으로 추정하지 말고 실제 모델 아키텍처를 기준으로 측정해야 합니다.

다중모달 융합에는 별도의 자원 예산이 필요합니다. 카메라, 라이다, 레이더, 지도(Map), 고유수용감각(Proprioceptive) 특징은 좌표 변환(Coordinate Transformation), 시간 정렬(Temporal Alignment), 버퍼링, 그리고 조감도(Bird\'s-Eye View, BEV), 복셀(Voxel), 점유(Occupancy), 잠재 공간(Latent Space)과 같은 공유 표현(Shared Representation)을 필요로 할 수 있습니다. 융합 단계 자체가 메모리와 연산의 주요 소비자가 될 수 있으며, 그 비용은 표현 크기, 융합 주파수(Fusion Frequency), 시간 이력 길이(Temporal History Length), 모달리티 간 상호작용 메커니즘에 따라 달라집니다.

시간적 AI(Temporal AI)는 추가적인 지속 자원 요구사항을 발생시킵니다. 추적, 순환 신경망(Recurrent Network), 시간 트랜스포머(Temporal Transformer), 점유 예측(Occupancy Forecasting), 월드 모델은 이전 관측값의 정보를 유지합니다. 더 긴 시간적 문맥(Temporal Context)은 예측 성능을 향상시킬 수 있지만 메모리 용량, 메모리 대역폭, 연산량을 증가시킵니다. 따라서 예측 지평과 이력 길이(History Length)는 단순한 AI 하이퍼파라미터(Hyperparameter)가 아니라 자원 예산의 명시적인 항목으로 포함해야 합니다.

이동형 로봇(Mobile Robot)에서는 전력 예산화(Power Budgeting)를 컴퓨팅 예산화와 분리할 수 없습니다. 센서, 네트워크 스위치, CPU, GPU, 가속기(Accelerator), 저장 장치, 냉각 시스템(Cooling System), 통신 장치는 모두 동일한 플랫폼의 에너지를 소비합니다. 연산 요구사항을 만족하더라도 지나치게 많은 전력을 소비하는 구성은 임무 지속시간(Mission Duration)을 감소시키거나 배터리 능력을 초과할 수 있습니다. 따라서 센서-AI 공동설계(Sensor-AI Co-Design)는 연산 선택을 TOPS 또는 FLOPS뿐만 아니라 와트(Watt)와 임무당 에너지(Energy per Mission)로도 표현해야 합니다.

열 용량(Thermal Capacity)은 또 다른 실질적인 제한 요소입니다. GPU와 프로세서는 짧은 벤치마크 워크로드에서는 요구 성능을 만족하더라도 지속적인 열 부하에서는 동작 주파수를 낮출 수 있습니다. 센서와 네트워킹 장비도 밀폐된 컴퓨팅 공간 내부에서 열을 발생시킵니다. 따라서 자원 검증(Resource Validation)은 현실적인 주변 환경 조건에서 지속적인 대표 워크로드를 사용해야 합니다. 일시적인 최대 성능에 기반한 컴퓨팅 예산은 장시간 임무에서 열 스로틀링(Thermal Throttling)이 사용 가능한 처리 능력을 감소시키면 더 이상 유효하지 않을 수 있습니다.

저장장치와 로깅(Logging)도 특히 개발, 검증, 데이터셋 수집(Dataset Collection) 과정에서 고려해야 합니다. 여러 개의 원시 카메라 스트림, 라이다 포인트 클라우드, 레이더 데이터, IMU 측정값, 로봇 상태, AI 출력을 기록하면 매우 큰 저장 용량이 필요할 수 있습니다. 디스크 처리량(Disk Throughput)이 추론 트래픽과 시스템 자원을 놓고 경쟁할 수도 있습니다. 따라서 실제 운용 시스템에서는 원시 데이터를 계속 저장하는 대신 선택적 로깅(Selective Logging), 압축(Compression), 이벤트 기반 기록(Event-Triggered Recording), 순환 버퍼(Rolling Buffer)를 사용할 수 있습니다.

실용적인 예산표(Budgeting Table)는 각각의 센싱 파이프라인에 원시 데이터 전송률(Raw Data Rate), 처리 후 데이터 전송률(Processed Data Rate), 메모리 사용량(Memory Footprint), 전처리 부하, 추론 부하, 지연시간, 전력, 예상 최대 사용률(Expected Peak Utilization)을 할당할 수 있습니다. 이후 네트워크 링크, CPU, GPU, 메모리 버스(Memory Bus), 저장장치와 같은 공유 자원의 사용량을 동시성(Concurrency)을 고려하여 합산해야 합니다. 이를 통해 추상적인 아키텍처를 하드웨어 통합 이전에 검토할 수 있는 정량적 자원 모델(Quantitative Resource Model)로 변환할 수 있습니다.

개발 과정에서 측정되는 워크로드는 계속 변화하기 때문에 자원 여유(Resource Margin)는 필수적입니다. AI 모델이 커지고, 추가적인 안전 기능이 등장하며, 로깅이 증가하고, 소프트웨어 오버헤드(Software Overhead)가 변화할 수 있습니다. 하드웨어를 100%에 가까운 사용률로 동작하도록 설계하면 이러한 변화나 예상하지 못한 조건에 대응할 여유가 거의 없습니다. 따라서 컴퓨팅, 대역폭, 메모리, 전력, 열 예산에는 사용되지 않는 용량을 낭비로 간주하지 말고 명시적인 엔지니어링 여유(Engineering Margin)를 확보해야 합니다.

자원에 제약이 발생했을 때의 우선순위도 예산화를 통해 정의할 수 있습니다. 안전 핵심 위치 추정(Safety-Critical Localization), 장애물 탐지, 상태 추정(State Estimation), 제어에는 보장된 컴퓨팅 및 통신 자원을 제공하고, 부가적인 분석이나 고비용 의미 기능(Semantic Function)은 축소할 수 있습니다. 스케줄링 정책(Scheduling Policy)을 통해 이러한 필수 파이프라인을 다른 경쟁 워크로드로부터 보호할 수 있습니다. 따라서 자원 할당(Resource Allocation)은 단순한 성능 최적화가 아니라 기능 안전(Functional Safety) 및 성능 저하 운용(Degraded Operation) 설계의 일부가 됩니다.

적응형 센싱(Adaptive Sensing)은 사용되지 않는 자원을 동적으로 활용할 수 있습니다. 예측 가능하고 위험도가 낮은 운용에서는 일부 센서나 AI 모델을 낮은 해상도, 낮은 주기 또는 낮은 복잡도로 동작시켜 평균 전력과 연산 소비를 감소시킬 수 있습니다. 불확실성, 속도, 지형 난이도, 환경 복잡도가 증가하면 추가적인 자원을 활성화할 수 있습니다. 최대 구성은 여전히 검증된 최대 한계(Validated Peak Limit) 안에 있어야 하지만 평균적인 운용 효율은 크게 향상될 수 있습니다.

이론적인 계산만으로는 모든 구현 효과를 반영할 수 없기 때문에 목표 하드웨어(Target Hardware)에서의 프로파일링(Profiling)이 필요합니다. GPU 커널(GPU Kernel), 메모리 할당, 장치 드라이버(Device Driver), 운영체제 스케줄링(Operating-System Scheduling), 네트워크 스택(Network Stack), 동기화, 모델 프레임워크(Model Framework), 하드웨어 가속기는 분석적 추정에서 놓칠 수 있는 오버헤드를 발생시킵니다. 따라서 대표적인 센서 기록과 현실적인 AI 워크로드를 실제 목표 컴퓨팅 플랫폼에서 재생하면서 처리량, 지연시간, 사용률, 메모리, 전력, 온도를 측정해야 합니다.

하드웨어 인 더 루프 시험(Hardware-in-the-Loop Testing)은 실제 센서 타이밍과 제어 상호작용을 재현하여 이러한 검증을 확장할 수 있습니다. 인공적으로 생성된 데이터 스트림에서는 실제 로봇에서 발생하는 버스트 패턴, 동기화 동작, 패킷 손실, 장치 드라이버 경쟁(Device-Driver Contention)이 나타나지 않을 수 있습니다. 현실적인 센서와 컴퓨팅 하드웨어를 이용하여 전체 아키텍처를 시험하면 현장 배치 이전에 병목현상을 발견하고 모든 시스템이 동시에 동작할 때에도 확보된 자원 여유가 유지되는지를 검증할 수 있습니다.

예산 위반(Budget Violation)은 운용 중에도 관측할 수 있어야 합니다. 시스템은 링크 사용률(Link Utilization), 메모리 압력(Memory Pressure), GPU 및 CPU 부하, 열 상태(Thermal State), 전력 소비, 큐 깊이, 프레임 손실, 추론 시간(Inference Duration), 측정 데이터 연령을 모니터링할 수 있습니다. 사전에 정의된 임계값을 초과하면 워크로드 감소, 센서 주기 조정, 모델 전환(Model Switching), 속도 감소 또는 성능 저하 운용을 실행할 수 있습니다. 따라서 자원 관리(Resource Management)는 자율 시스템 동작의 능동적인 일부가 됩니다.

센서-컴퓨팅-대역폭 예산화는 궁극적으로 임무 수준 성능(Task-Level Performance)을 기준으로 평가해야 합니다. 이미지 해상도나 처리 주파수를 낮추면 로봇 행동에는 거의 영향을 주지 않으면서 상당한 자원을 절약할 수도 있지만, 다른 경우에는 작은 감소만으로도 장애물 탐지나 위치 추정 성능이 급격히 저하될 수 있습니다. 설계 반복 과정에서는 자원 절감량과 AI 능력 손실(AI Capability Loss)을 모두 측정해야 합니다. 목표는 신뢰성 있는 물리적 지능(Physical Intelligence)에 필요한 정보를 제거하지 않으면서 불필요한 연산 요구량을 줄이는 것입니다.

따라서 최종 예산(Final Budget)은 센서와 컴퓨터를 선택한 이후 한 번 작성하고 끝나는 정적인 스프레드시트(Static Spreadsheet)가 아닙니다. 이는 센서 구성, AI 아키텍처, 통신, 메모리, 지연시간, 전력, 열 특성, 로봇 동역학(Robot Dynamics)을 연결하는 지속적으로 갱신되는 엔지니어링 모델(Living Engineering Model)입니다. 모델과 하드웨어가 발전함에 따라 실제 측정 결과를 예산에 반영하여 시스템이 충분한 여유를 확보한 상태에서 검증된 운용 범위(Validated Operating Envelope) 안에 계속 존재하는지를 확인해야 합니다.

센서-컴퓨팅-대역폭 예산화(Sensor-Compute-Bandwidth Budgeting)는 정성적인 센싱 선택을 측정 가능한 시스템 제약조건으로 변환함으로써 센서-AI 공동설계(Sensor-AI Co-Design)를 완성합니다. 핵심 목표는 균형 잡힌 정보 흐름(Balanced Information Flow)입니다. 중요한 물리 상태를 관측 가능하게 만들 만큼 충분한 센싱, 관측값을 유용한 지능으로 변환할 만큼 충분한 컴퓨팅, 그리고 실시간 마감시간을 위반하지 않고 정보를 이동시킬 만큼 충분한 대역폭이 필요합니다. 강건한 피지컬 AI(Robust Physical AI)는 센싱 능력과 컴퓨팅 능력을 각각 독립적으로 최대화하는 것이 아니라 정량적으로 서로 일치시킬 때 구현됩니다.
