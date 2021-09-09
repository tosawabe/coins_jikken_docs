# 中間課題(Gazebo)



### 実行方法

~~~
(端末1) $ roslaunch yamasemi_sim intro_2020.launch
入門課題用Gazeboのワールドを起動
~~~

~~~
(端末2) $ roslaunch beego_gazebo beego.launch
GazeboのワールドにBeegoを召喚

または、
(端末2) $ roslaunch beego_gazebo beego.launch x:=3.0 y:=-2.0
（デフォルトではx:=0.0，y:=0.0，z:=0.0）
(端末2) $ roslaunch beego_gazebo beego.launch Y:=3.14
（デフォルトではR:=0.0，P:=0.0，Y:=0.0）
このように初期姿勢を引数で指定できる
~~~

~~~
(端末3) $ rosrun coins_jikken mid_assignment
中間課題の命令を実行
~~~