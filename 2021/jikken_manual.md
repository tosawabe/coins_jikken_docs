# 移動ロボットの行動プログラミング

2021/08 

本実験では主に*~/catkin_ws/src/coins_jikken/coins_jikken/src*の中のプログラムをいじってもらいます。

必要に応じて新しい命令を追加するために*~/catkin_ws/src/coins_jikken/beego_controller*に変更を加えてください。

### 課題のやり方

サンプルコード これを参考に課題をすすめる (穴埋めあり)

~~~
~/catkin_ws/coins_jikken/coins_jikken/sample
|-	sample1.cpp
	sample2.cpp
	sample3.cpp
~~~

課題コード ここに記述して実験してください

~~~
~/catkin_ws/coins_jikken/coins_jikken/src
|-  mid_assignment.cpp	中間課題
	fin_assignment.cpp	最終課題
	cha_assignment.cpp	挑戦課題
~~~

コンパイル

~~~
$ cd ~/catkin_ws
$ catkin_make
~~~

## シミュレータ(gazebo)を利用して実行

~~~
(端末1) $ roslaunch yamasemi_sim (任意のworld).launch
Gazeboのワールドを起動
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

以上でシミュレータの準備は完了。次に命令を記述したcoins_jikkenパッケージのノードを起動します。

~~~
(端末3) $ rosrun coins_jikken sample1
サンプル1の命令を実行
~~~

- その他

  ~~~
  $ roslaunch yamasemi_sim final_2020.launch rviz:=false
  $ roslaunch beego_gazebo beego.launch rviz:=false
  rvizを起動しない
  ~~~


## 実機を利用して実行

~~~
ロボットに電源が入っており、パソコンとも接続できていることを確認
(電源の入れ方、ロボットの区別はその時に教えます)

beegoなら
(端末1) $ roslaunch coins_jikken beego.launch
speegoなら
(端末1) $ roslaunch coins_jikken speego.launch
M1なら
(端末1) $ roslaunch coins_jikken M1.launch

命令を記述したノードを起動
サンプル１
(端末2) $ rosrun coins_jikken sanple1
~~~

### 補足

- 新しくノードを作成する方法

  ~~~
  src
  |- 	beg_assignment.cpp	入門課題
  	mid_assignment.cpp	中間課題
  	fin_assignment.cpp	最終課題
  	cha_assignment.cpp	挑戦課題
  	test_coins.cpp		新しく作成したノード
  ~~~

  新しくノードを作成する場合、CMakeLists.txtを変更しないとコンパイルが出来ません。

  以下をcoins_jikken/coins_jikken/CMakeLists.txtに追加。

  ~~~
  add_executable(test_coins
  	src/test_coins.cpp
  	)
  target_link_libraries(test_coins
    	beego_controller
  	${catkin_LIBRARIES}
  	)
  ~~~

  ~~~cpp
  // test_coins.cppにて起動するときのノード名を設定
  ros::init(argc, argv, "test_coins_node");
  ~~~

  以下で実行

  ~~~
  $ rosrun coins_jikken test_coins_node
  ~~~

- 挑戦課題の一例

  ~~~
  $ roslaunch coins_jikken karugamo_sim.launch
  ~~~

  二台のロボットが出現。眼の前のロボットを親として、その親に追従する子のプログラムの作成。

  実行したターミナルをフォーカスしてキーボードの十字キーで親ロボットを動かせる。

- ロボットの速度・角速度を変更

  以下を変更

  ~~~c++
  # ~/catkin_ws/src/coins_jikken/beego_controller/include/beego_controller/beego_controller.h
  
  #define POS_TOLERANCE 0.001        // 状態遷移時に許容する位置誤差 [m]
  #define RAD_TOLERANCE M_PI / 180.0 // 状態遷移時に許容する角度誤差 [rad]
  #define LINEAR_VEL 0.15            // ロボットの並進速度 [m/s]
  #define ANGULAR_VEL 0.5            // ロボットの角速度 [rad/s]
  ~~~

  

## Stage

Gazebo、実機の他にStageというシミュレータがある。

sample2,3をやる際に利用できるかもしれない。

~~~
$ roslaunch coins_jikken stage.launch
$ roslaunch coins_jikken teleop_key.launch
~~~

## Tips

Ubuntuのバージョンによってはちょっと違うかもしれません......

- terminator

  コマンドを実行するためのコマンドラインインターフェース。同じウィンドウ内で画面を複数分割できるため便利。

  ~~~
  $ sudo apt install terminator
  インストール
  ~~~

  | 操作     | キー             |
  | -------- | ---------------- |
  | 水平分割 | Ctrl + Shift + o |
  | 垂直分割 | Ctrl + Shift + e |

  好きなレイアウトを設定したら、その設定を保存できる。

  ~~~
  1. ターミナルを右クリック
  2. 設定をクリック
  3. タブからレイアウトを選択
  4. 「追加(A)」から新しいレイアウトを増やし名前をつける(ここの例では「ros」)
  
  $ terminator -l ros&exit
  とすればいつでも同じレイアウトで起動できる。
  ~~~

- スクリーンショット

  ~~~
  $ gnome-screenshot --area
  ~~~

- エイリアス

  以上紹介したコマンドは少し長くめんどくさいのでエイリアスを設定しましょう。

  ~~~
  $ sudo gedit ~/.bashrc
  ~~~

  .bashrcのファイル内に以下の文を追加

  ~~~bash
  # .bashrc
  
  alias rosterminator='terminator -l ros&exit'
  alias s='gnome-screenshot --area'
  ~~~

  ~~~
  $ source ~/.bashrc
  .bashrcの再読み込み
  
  $ rosterminator
  $ s
  以上のコマンドで同様のことができるようになる
  ~~~

  [本当はこう](https://qiita.com/miriwo/items/0a3bce6bb04aaca6a7a6)