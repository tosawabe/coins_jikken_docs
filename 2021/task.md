# 課題

2021/8 更新

### コンパイル

~~~
$ cd ~/catkin_ws
$ catkin_make
~~~

## (1) sample1を実行しロボットを動かす

BeegoControllerを利用して前進・旋回の操作を確認

- シミュレータ

  ~~~
  (端末1) $ roslaunch yamasemi_sim intro_2020.launch
  (端末2) $ roslaunch beego_gazebo beego.launch
  (端末3) $ rosrun coins_jikken sample1
  ~~~

- 実機 (beegoの場合 以下ここではbeegoを使用しているとする)

  実機を使う前に[山彦ロボットについて(実機での実験用)](https://www.roboken.iit.tsukuba.ac.jp/lectures/software_science_experiment/2019/document/robot_hardware.pdf)に目を通す。

  ~~~
  (端末1) $ roslaunch coins_jikken beego.launch
  (端末2) $ rosrun coins_jikken sample1
  ~~~

### 課題1

サンプルコードの内容を変更し、1m✕1mの正方形の軌跡を描くようにロボットを動作させる。

## (2) samle2を実行しセンサの扱い方の確認

URGによる点群を取得する

- シミュレータ

  ~~~
  (端末1) $ roslaunch yamasemi_sim intro_2020.launch
  (端末2) $ roslaunch beego_gazebo beego.launch
      (端末2) $ roslaunch beego_gazebo beego.launch x:=3.0 y:=-2.0
      （デフォルトではx:=0.0，y:=0.0，z:=0.0）
      (端末2) $ roslaunch beego_gazebo beego.launch Y:=3.14
      （デフォルトではR:=0.0，P:=0.0，Y:=0.0）
  	このように初期姿勢を引数で指定できる
  (端末3) $ rosrun coins_jikken sample2
  ~~~

- 実機

  ~~~
  (端末1) $ roslaunch coins_jikken beego.launch
  (端末2) $ rosrun coins_jikken sample2
  ~~~

sample2ではロボット正面から障害物までの距離測っており、その距離が1m以下の場合停止する。

### 課題2

sample2.cpp内の以下のiを変更し、ロボットの真右方向の障害物までの距離が1mの以下の場合停止するようにする。

参考画像->[スキャンデータ補足画像]()

~~~cpp
int i = -scan.angle_min / scan.angle_increment;
~~~

実際にロボットを動作させロボットが停止すること確認する。TAにも見てもらう。

## (3) sample3を実行し簡単な地図を作る

座標変換の確認

- シミュレータ

  ~~~
  (端末1) $ roslaunch yamasemi_sim intro_2020.launch
  (端末2) $ roslaunch beego_gazebo beego.launch
  (端末3) $ rosrun coins_jikken sample3
  (端末4) $ roslaunch coins_jikken teleop_key.launch
  端末4をフォーカスしながら十字キーでロボットを操作することができる
  一通り走ったら端末3をCntl+cで終了し
  odom.txt, map.txtが保存されていることを確認
  ~~~

- 実機

  ~~~
  (端末1) $ roslaunch coins_jikken beego.launch
  (端末2) $ rosrun coins_jikken sample3
  (端末3) $ roslaunch coins_jikken teleop_key.launch
  端末3をフォーカスしながら十字キーでロボットを操作することができる
  実機は手で押しても可
  一通り走ったら端末3をCntl+cで終了し
  odom.txt, map.txtが保存されていることを確認
  ~~~

odom.txtには一秒ごとのロボットのGL座標系でのx,yがmap.txtにはGL座標系でのロボットが取得した点群それぞれのx,yが保存される。

gnuplotなどでプロットしてみよう

~~~
gnuplotで表示
$ gnuplot
gnuplot> plot "odom.txt", "map.txt"
gnuplot> exit

オプションなどは適当に調べてみよう
~~~

~~~
簡単にpythonのmatplotlibで表示してみるサンプルコード
(plot.pyとして用意した)
$ cd ~/catkin_ws/src/coins_jikken/coins_jikken
$ python3 plot.py

plot.pyでの読み込むファイルのパスは確認し、正しいものにする。
~~~

### 課題3

しかし、このままでは地図はできない

sample3.cpp内の以下の内容を変更する。[オドメトリと座標系(sample3参考資料)](https://www.roboken.iit.tsukuba.ac.jp/lectures/software_science_experiment/2019/document/odometry_and_coordinate.pdf)を参考に、以下の二行を変更する。

~~~cpp
// FS座標系からGL座標系への変換
// (参考) /base_link -> /odom
x_gl = 0;
y_gl = 0;
~~~

うまく地図っぽいのができたか確認する。TAにも見てもらう。
