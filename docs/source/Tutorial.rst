チュートリアル
===================================

本章ではタスク管理機構を使用して建設機械を操作する流れを説明します。
ROS2-TMS for Constructionでは以下に示すいくつかのサンプルタスクが用意されています。


.. image:: images/sample_tasks.png
   :alt: OPERAのアーキテクチャ
   :width: 800px
   :align: center  
  
.. raw:: html

   <br><br>

これらのタスクは共通制御信号対応の建設機械の実機に加え、OperaSim-PhysX/AGX上の建設機械を操作することが可能です。
これらのタスクはデータベース上の~/rostmsdb/task以下に格納されおります。
また、xml形式のタスク列については `こちら <https://github.com/irvs/ros2_tms_for_construction/tree/develop/ts/tms_ts/tms_ts_manager/config/official_registred_tasks>`_ 
に置いてあります。

なお、以下を実行する前に必ず `こちら <https://github.com/pwri-opera/OperaSim-PhysX>`_ の手順にしたがって
OperaSim-PhysXとUbuntu22.04 PC間の接続ができることを確認し、OperaSim-PshyXのシミュレーションを起動した状態で以下のコマンドを実行してください。


  1つめのターミナル::

    cd ~/ros2-tms-for-construction_ws && source install/setup.bash
    ros2 launch ros_tcp_endpoint endpoint.py

  2つめのターミナル::

    cd ~/ros2-tms-for-construction_ws && source install/setup.bash
    ros2 launch zx200_bringup vehicle.launch.py command_interface_name:=velocity use_rviz:=true

  3つめのターミナル::

    cd ~/ros2-tms-for-construction_ws && source install/setup.bash
    ros2 launch tms_if_for_opera tms_if_for_opera.launch.py

  4つめのターミナル::
  
    cd ~/ros2-tms-for-construction_ws && source install/setup.bash
    ros2 launch ic120_unity ic120_standby_ekf.launch.py  
  
  5つめのターミナル::

    cd ~/ros2-tms-for-construction_ws && source install/setup.bash
    ros2 launch tms_ts_launch tms_ts_construction.launch.py task_id:=<task_id>
  
  ※ <task_id>には1,2,3,4のいずれかの値を指定してください

実行するタスクに応じて以下に示すとおりOperaSim-PhysX上の建設機械が動作する様子が見て取れると思います。

// タスク1,2,3,4実行時の動画を個々に貼り付け

上記のサンプルサブタスクは :doc:`こちら <TaskManagementMechanism>` に示す手順にしたがって、既存のサンプルタスクをもとに別のタスクを構築したり、タスクを実行している様子を可視化することが可能です。


