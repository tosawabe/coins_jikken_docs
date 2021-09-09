# beego_controller

2021/08更新

gazebo上でbeegoモデルを操作するための関数を用意しています。

詳しい使用方法はcoins_jikken/sampleのサンプルコード参照

## コマンド一覧

- straight

  直進命令のcmd_velトピックをパブリッシュ

  ロボットが直進

- spinRight

  右回転のcmd_velトピックをパブリッシュ

  ロボットが右旋回

- spinLeft

  左回転のcmd_velトピックをパブリッシュ

  ロボットが左旋回

- stop

  すべてのパラメータが0(停止)のcmd_velトピックをパブリッシュ

  ロボットが停止

- control(x, yaw)

  並進速度x m/s, 角速度yaw rad/sで移動

  straightやspinRight等と同様だが、これは速度を指定できる

- calcYaw

  geometry_msgs型のposeを受け取りYaw角を計算し返す

  現在のロボットの角度を取得

- updateReferencePose

  現在のロボットのposeを引数へ保存

- getCurrentPose

  現在のロボットのposeを引数へ保存

  updateReferencePoseと動作は変わりません。便宜上分けています。

- getCurrentScan

  現在のスキャナの取得結果をsensor_msgs::LaserScanの引数に渡す

  現在のロボットのスキャンデータを受け取る

- normalize_angle

  角度を正規化

- saveFootmark

  軌跡をファイルに保存

## 補足

- 状態遷移時に許容する誤差や、ロボットの速度・角速度は以下から変更可

  ~~~cpp
  # include/beego_controller/beego_controller.h
  
  #define POS_TOLERANCE 0.001        // 状態遷移時に許容する位置誤差 [m]
  #define RAD_TOLERANCE M_PI / 180.0 // 状態遷移時に許容する角度誤差 [rad]
  #define LINEAR_VEL 0.15            // ロボットの並進速度 [m/s]
  #define ANGULAR_VEL 0.5            // ロボットの角速度 [rad/s]
  ~~~

