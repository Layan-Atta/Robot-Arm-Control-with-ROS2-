# Robot Arm Control with ROS2 🤖

## نظرة عامة
مشروع للتحكم بذراع روبوت باستخدام ROS2 Humble، يتضمن التحكم اليدوي عبر Joint State Publisher والتحكم الذكي باستخدام MoveIt مع دعم الحركيات العكسية (Inverse Kinematics).

---

## المتطلبات الأساسية

### البيئة
- **نظام التشغيل**: Ubuntu 22.04
- **ROS Version**: ROS2 Humble
- **منصة التطوير**: TheConstruct (أو أي بيئة ROS2 محلية)

### المكتبات المطلوبة
```bash
ros-humble-joint-state-publisher-gui
ros-humble-gazebo-ros
ros-humble-xacro
ros-humble-ros2-control
ros-humble-ros2-controllers
ros-humble-joint-state-broadcaster
ros-humble-joint-trajectory-controller
ros-humble-controller-manager
ros-humble-moveit
ros-humble-gazebo-ros2-control
```

---

## التثبيت والإعداد

### الخطوة 1: إنشاء Workspace
```bash
mkdir -p ~/ros2_ws/src
cd ~/ros2_ws/src
```

### الخطوة 2: استنساخ المشروع
```bash
git clone https://github.com/smart-methods/Robot_Arm_ROS2.git
cd ~/ros2_ws
```

### الخطوة 3: تثبيت Dependencies
```bash
sudo apt-get update && sudo apt-get install -y \
     ros-humble-joint-state-publisher-gui \
     ros-humble-gazebo-ros \
     ros-humble-xacro \
     ros-humble-ros2-control \
     ros-humble-ros2-controllers \
     ros-humble-joint-state-broadcaster \
     ros-humble-joint-trajectory-controller \
     ros-humble-controller-manager \
     ros-humble-moveit \
     ros-humble-gazebo-ros2-control
```

### الخطوة 4: Build المشروع
```bash
cd ~/ros2_ws
colcon build
source install/setup.bash
```

---

## Task 1: التحكم بالروبوت باستخدام Joint State Publisher

### الوصف
التحكم اليدوي بمفاصل الروبوت باستخدام واجهة رسومية (GUI) تسمح بتحريك كل مفصل بشكل مستقل.

### تشغيل المهمة
```bash
source ~/ros2_ws/install/setup.bash
ros2 launch arduinobot_description display.launch.xml
```

### النتائج
- يفتح برنامج **RViz** لعرض الروبوت ثلاثي الأبعاد
- يفتح **Joint State Publisher GUI** للتحكم بالمفاصل
- يمكن تحريك المفاصل باستخدام الـ sliders

### إعدادات مهمة في RViz
لعرض الروبوت بشكل صحيح، تأكد من:
1. إضافة **RobotModel** من قائمة Add
2. تعيين **Fixed Frame** إلى `base_link`
3. تعيين **Description Topic** إلى `/robot_description`

### الصعوبات والحلول
| المشكلة | الحل |
|---------|------|
| الروبوت لا يظهر في RViz | إضافة RobotModel يدوياً وتعيين Fixed Frame إلى base_link |
| شكل أبيض فقط يظهر | تغيير Fixed Frame من world إلى base_link |

### Screenshots

#### الوضع الافتراضي للروبوت
![Task 1 - Initial Position](screenshots/task1_initial_position.png)
*الصورة 3: الروبوت في الوضع الابتدائي مع Joint State Publisher GUI*

#### حركة الروبوت - وضع مختلف
![Task 1 - Different Position](screenshots/task1_different_position.png)
*الصورة 4: الروبوت بعد تحريك المفاصل باستخدام الـ sliders*

### ملاحظات Task 1
- جميع المفاصل (joint_1 إلى joint_4) قابلة للتحكم
- التحكم في الزمن الحقيقي (Real-time control)
- التصور ثلاثي الأبعاد في RViz يتحدث فورياً

---

## Task 2: التحكم بالروبوت باستخدام MoveIt

### الوصف
استخدام MoveIt للتخطيط وتنفيذ حركات معقدة للروبوت بالاعتماد على الحركيات العكسية (Inverse Kinematics) وتجنب الاصطدامات.

### تشغيل المهمة

#### الطريقة المستخدمة (بدون Gazebo):
```bash
source ~/ros2_ws/install/setup.bash
ros2 launch arduinobot_mc demo.launch.py
```

#### الطريقة البديلة (مع Gazebo Simulation):
**Terminal 1:**
```bash
source ~/ros2_ws/install/setup.bash
ros2 launch arduinobot_description simulation.launch.py
```

**Terminal 2:**
```bash
source ~/ros2_ws/install/setup.bash
ros2 launch arduinobot_mc demo.launch.py
```

### خطوات الاستخدام

1. **تحديد Planning Group**
   - في واجهة MoveIt، تأكد من اختيار المجموعة الصحيحة (عادة `arm` أو `manipulator`)

2. **تحريك End Effector**
   - اسحب الكرة التفاعلية (Interactive Marker) إلى الموقع المطلوب
   - استخدم الفأرة لتحريكها في جميع الاتجاهات

3. **تخطيط الحركة**
   - اضغط على زر **"Plan"** في لوحة MotionPlanning
   - سيعرض المسار المخطط للحركة

4. **تنفيذ الحركة**
   - اضغط على **"Execute"** لتنفيذ الحركة المخططة
   - يمكن استخدام **"Plan and Execute"** مباشرة

### الإعدادات المستخدمة
- ✅ **Allow Collision** (Approximate IK Solution): مفعّل
- ✅ **Collision Checking**: مفعّل لتجنب الاصطدامات

### المميزات
- 🎯 تخطيط مسارات آمنة وخالية من الاصطدامات
- 🔄 حساب الحركيات العكسية تلقائياً
- 📊 تصور المسار قبل التنفيذ
- ⚡ تنفيذ سلس للحركات المعقدة

### Screenshots
<!-- أضف screenshots هنا -->
![Task 2 - MoveIt Interface](screenshots/task2_moveit_interface.png)
![Task 2 - Planning](screenshots/task2_planning.png)
![Task 2 - Execution](screenshots/task2_execution.png)

---

## البنية الهيكلية للمشروع

```
Robot_Arm_ROS2/
├── arduinobot_description/     # وصف الروبوت (URDF/Xacro)
│   ├── urdf/                   # ملفات تعريف الروبوت
│   ├── meshes/                 # نماذج 3D
│   ├── launch/                 # ملفات Launch
│   └── rviz/                   # إعدادات RViz
├── arduinobot_controller/      # متحكمات ROS2 Control
└── arduinobot_mc/              # إعدادات MoveIt Configuration
    ├── config/                 # ملفات إعدادات MoveIt
    └── launch/                 # ملفات Launch لـ MoveIt
```

---

## الأوامر المفيدة

### فحص الـ Topics
```bash
ros2 topic list
ros2 topic echo /joint_states
```

### فحص الـ TF Tree
```bash
ros2 run tf2_tools view_frames
```

### إعادة Build
```bash
cd ~/ros2_ws
rm -rf build install log
colcon build
source install/setup.bash
```

---

## التعلم والاستنتاجات

### ما تم تعلمه
1. **Joint State Publisher**: فهم كيفية التحكم اليدوي بمفاصل الروبوت
2. **RViz Configuration**: أهمية إعداد Fixed Frame بشكل صحيح
3. **MoveIt**: استخدام أدوات التخطيط المتقدمة للحركة
4. **Inverse Kinematics**: كيف يحسب MoveIt زوايا المفاصل تلقائياً

### الصعوبات التي تم حلها
- مشكلة عدم ظهور الروبوت في RViz → الحل: تعيين Fixed Frame إلى base_link
- فهم واجهة MoveIt → الحل: التجربة والاستكشاف

### التطويرات المستقبلية
- [ ] إضافة سيناريوهات حركة معقدة
- [ ] التحكم بالروبوت الفعلي (Hardware)
- [ ] إضافة Computer Vision للتقاط الأشياء
- [ ] تطوير واجهة مخصصة للتحكم

---

## المراجع والمصادر

- [المشروع الأصلي - Smart Methods](https://github.com/smart-methods/Robot_Arm_ROS2)
- [ROS2 Humble Documentation](https://docs.ros.org/en/humble/)
- [MoveIt2 Documentation](https://moveit.picknik.ai/)
- [TheConstruct Academy](https://www.theconstructsim.com/)

---

## المطور

**LayanAtta**


---

## الترخيص

هذا المشروع مبني على [Robot_Arm_ROS2](https://github.com/smart-methods/Robot_Arm_ROS2) من Smart Methods.

---

## شكر وتقدير

شكر خاص لـ **Smart Methods** على توفير المشروع الأساسي وللمهندس Baraa Aljouri على إرشادة، و**TheConstruct** على منصة التطوير السحابية.
