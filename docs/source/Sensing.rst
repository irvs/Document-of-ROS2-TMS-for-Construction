センシング機能について
===================================

ROS2-TMS for Cinstructionのタスク管理機構では、外部のセンシングシステムから値を受け取り、
その値に応じて建設機械の動作を変化させることが可能です。
その概要図を以下に示します。

    .. image:: images/system_architecture.png
      :alt: システムの全体像
      :width: 600px
      :align: center  

    .. raw:: html

        <br><br>

上記では、タスクスケジューラがデータベースを介して、環境側に配置されたセンサからの情報を受け取り、その値に応じて建設機械を動作させる様子を示しています。
大まかな動作の流れはこちらで説明したとおりです。
本章では特に上図の赤枠で囲った、ROS2-TMS for Constructionが環境配置型センサからの情報を受け取る部分について説明します。
なお、本機能は以下のモジュール図の赤枠で囲ったTMS_SPに相当する部分です。



TMS_SPは「センシングシステム及びROS2-TMS for Costruction間通信のデータ型」・「受信したデータをROS2-TMS for Constrctionのデータベースに書き込むプログラム」
から構成されます。従来これらの機能はこちらのROS2-TMS for Constructionのリポジトリ内で管理されていたものの、センシングシステムの仕様に依存している一面もあり、
こちらのパッケージに分けることとしました。

.. note::
   
   TMS_SPの性質上、センシングシステム側の仕様に合わせてユーザ側でプログラムの実装が必要です。
   実装に際しては以下のサンプルプログラムを参考にしてください。


サンプルプログラムの実行方法
-----------------------------

1. まずはじめに以下を実行し、ROS2トピックを受け取ってデータベースに値を書き込むためのROS2ノードを起動します。::
   
   cd ~/ros2-tms-for-construction_ws
   colcon build --packages-select sensing_msgs tms_sp_sensing && source install/setup.bash
   ros2 run tms_sp_sensing sample
   
2. 次に以下のコマンドを実行し、値を書き込むためのノードを実行します。::
   
   cd 
   mkdir -p sensing_ws/src
   cd sensing_ws/src
   git clone -b master https://github.com/kasahara-san/sensing_sample_cps.git
   cd ..
   colcon build --packages-select sample_sensing_nodes sensing_msgs && source install/setup.bash
   ros2 run sample_sensing_nodes sample_publisher

3. そしてMongoDB Compassを起動し、~/rostmsdb/parameter下の以下のデータを確認すると、値が更新されていく様子が確認できます。
   
    .. image:: images/dynamic_parameters_imp.png
       :alt: 動的パラメータが更新されていく様子の可視化
       :width: 600px
       :align: center  

    .. raw:: html

        <br><br>

上記のようにデータベースの値をセンシングシステムから送られてきた値で上書きすることで、Behavior Treeは常に最新の情報をデータベースから読み出してくることが可能
となります。なお、このようにタスク実行中にリアルタイムに更新されるデータベース上のデータのことを動的パラメータデータと呼んでいます。





