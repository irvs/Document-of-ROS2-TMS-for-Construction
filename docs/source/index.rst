ROS2-TMS for Constructionの概要
===================================

Overview
-----------------------------------

ROS2-TMS for Constructionは九州大学 倉爪研究室で開発が進められている工事現場向けのCyber Physical System(CPS)です。
本システムでは工事現場に配置したセンサから様々な情報を収集し、以下の図に示すCyber Physical Systemの
構造に基づいて以下の手順で処理した上で、工事現場に配置された建設機械へと動作指令を送ることで自律化施工を行います。

.. image:: images/System_Concept.svg
   :alt: システムの概念図
   :width: 400px
   :align: center  
  

1. 工事現場の様々な箇所や自律移動建設機械に設置されたセンサからの情報を収集し、データベースに蓄えます。ここで蓄えた情報を環境情報と呼びます (Measure)
2. 環境情報をもとにサイバー空間上にリアルタイムな工事現場を構築します。その後、サイバー空間上の工事現場を分析し、建設機械が次に取るべき動作の計画を行います (Analyze,Plan)
3. 先程計画した動作に基づき、これを実現するために必要な制御信号を計算し、建設機械へと送ります (Control)

資金提供
--------------------------------------

本研究の一部はJST【ムーンショット型研究開発事業】グラント番号【JPMJMS2032】及び内閣府総合科学技術・イノベーション会議の戦略的イノベーション創造プログラム（SIP) 
第３期「 スマートインフラマネジメントシステムの構築 」JPJ012187（研究推進法人: 土木研究所）の資金提供を受け、実施されました。

実装背景
--------------------------------------
現在、土木研究所は建設機械の自動化に用いる信号の共通規格化を推進しており、この原案を”  `共通制御信号 <https://www.pwri.go.jp/team/advanced/pdf/seigyosingou.pdf>`_ ”と呼びます。
共通制御信号は単なる信号案であり、これを実装した機体と、その自動化に必要となるアプリケーションがなければ建設機械の自律化はできません。
そこで、土木研究所は共通制御信号の実証プラットフォームとして  `自律化施工基盤 <https://www.pwri.go.jp/team/advanced/opera.html>`_ を実装しました。
自律化施工基盤OPERAには以下に示すとおり、建設機械の実機・シミュレータからナビゲーション・マニピュレーションといったアプリケーションまでが実装され、GitHub上で一般公開されています。
( OPERA's Scopeの赤枠内部が自律化施工基盤OPERAの実装範囲です )

.. image:: images/OPERA_architecture.png
   :alt: OPERAのアーキテクチャ
   :width: 400px
   :align: center  
  

その一方で、自律化施工基盤OPERAのみでは自律化施工を実現することはできません。
ナビゲーションやアプリケーションというのは目標となる位置姿勢を与えて初めて自律動作するものです。
自律化施工というのは「現場の状況を逐次把握しつつ、状況に応じた動作指令を建設機械へと送る機能」が必要であり、これを担うのが状態機械や行動木といったタスクスケジューラです。
したがって、我々は自律化施工を実現するため、自律化施工基盤OPERAの上にタスクスケジューリング機能を実装した本システム「ROS2-TMS for Construction」を実装しました。

.. image:: images/scope_of_ros2-tms_for_const.png
   :alt: ROS2-TMS for Constructionのスコープ
   :width: 600px
   :align: center  
  

アーキテクチャ
--------------------------------------
ROS2-TMS for Constructionでは以下に示す通り、各々の機能ごとにモジュールが分割されており、各々のモジュールが必要に応じて通信を行うことで
上記のシステムを成立させています。

.. image:: images/System_Architecture.svg
   :alt: システム構造
   :width: 400px
   :align: center  
  

各々のモジュールの説明は以下に示すとおりです。

- **Database Mobule (TMS_DB)** :   
  
  データベースへの入出力機能を担うモジュール
- **User Rqquest Module (TMS_UR)** : 
  
  ユーザからの入力を受け付けるモジュール
- **Task Scheduler Module (TMS_TS)**: 
  
  与えられたタスク(施工シナリオ)をタスクスケジューラによって処理するモジュール
- **Interface Module (TMS_IF_for_OPERA)** :   
  
  ROS2-TMS for ConstructionとOPERAを接続するモジュール
- **Robot Planning Module (TMS_RP) [OPERA]**:
  
  TMS_TSからの指令をもとに建設機械の動作計画を行うモジュール
- **Robot Controller Module (TMS_RC) [OPERA]** : 
  
  TMS_RPからの指令をもとに建設機械への入力信号を計算するモジュール  なお、タスクの全体像は以下に示すとおりである。
- **Sensor Driver Module (TMS_SD)** :
  
  センサからの値を受け取り、そのままTMS_DBへ送るモジュール
- **Sensor System Module (TMS_SS)** : 
  
  センサからの値を加工してからTMS_DBに送るモジュール

なお、現在ROS2-TMS for Constructionは大きく以下の2つのシステムに分け、開発が進められています。個々の機能については各々別ページで紹介します。

- **タスク管理機構** : [ :doc:`こちらを参照ください <TaskManagementMechanism>`  ]
  
  タスクスケジューラ（Behavior Tree）を使用し、工事現場の状態を逐次把握しつつ、その時々の現場の状態に合わせて自律移動建設機械を動作させる機能
- **OperaSim-VR**  [ 作成中  ] 
  
  工事現場から得られる情報をもとにサイバー空間上にリアルタイムな工事現場を構築する機能


.. なお、ROS2-TMS for Constructionから操作可能な建設機械には以下のものが存在します。

.. .. image:: images/OPERA_machinaries.svg
..    :alt: OPERA対応建設機械
..    :width: 600px
..    :align: center  
  
.. .. raw:: html

   <br><br>

参考文献（国際会議・ジャーナル誌）
--------------------------------------

1. Yuichiro Kasahara, Tomoya Itsuka, Koshi Shibata, Tomoya Kouno, Ryuichi Maeda, Kohei Matsumoto, Shunsuke Kimura, Yutaro Fukase, 
   Takashi Yokoshima, Genki Yamauchi, Daisuke Endo, Takeshi Hashimoto, and Ryo Kurazume, `Task management system for construction machinery using the open platform OPERA <https://robotics.ait.kyushu-u.ac.jp/kurazume/papers/KasaharaROMAN24.pdf>`_, IEEE International Conference on Robot & Human Interactive Communication (RO-MAN), 2024

    .. code-block:: bibtex

       @inproceedings{kasahara2024roman,
         author    = {Yuichiro Kasahara, Tomoya Itsuka, Koshi Shibata, Tomoya Kouno, Ryuichi Maeda, Kohei Matsumoto, Shunsuke Kimura, Yutaro Fukase, 
                      Takashi Yokoshima, Genki Yamauchi, Daisuke Endo, Takeshi Hashimoto, and Ryo Kurazume},
         title     = {Task Management System for Construction Machinery Using the Open Platform OPERA},
         booktitle = {IEEE International Conference on Robot & Human Interactive Communication (RO-MAN)},
         year      = {2024},
       }


2. Ryuichi Maeda, Tomoya Kouno, Kohei Matsumoto, Yuichiro Kasahara, Tomoya Itsuka, Kazuto Nakashima, Yusuke Tamaishi and Ryo Kurazume,
   `Sensor Pods and ROS2-TMS for Construction for Cyber-Physical System at Earthwork Sites <https://robotics.ait.kyushu-u.ac.jp/kurazume/papers/MaedaSSRR24.pdf>`_, IEEE International Symposium on Safety, Security, and Rescue Robotics (SSRR), 2024

    .. code-block:: bibtex

       @inproceedings{maeda2024ssrr,
         author    = {Ryuichi Maeda, Tomoya Kouno, Kohei Matsumoto, Yuichiro Kasahara, Tomoya Itsuka, Kazuto Nakashima, Yusuke Tamaishi and Ryo Kurazume},
         title     = {Task Management System for Construction Machinery Using the Open Platform OPERA},
         booktitle = {IEEE International Symposium on Safety, Security, and Rescue Robotics (SSRR)},
         year      = {2024},
       }

3. Yuichiro Kasahara, Kota Akinari, Tomoya Kouno, Noriko Sano, Taro Abe, Genki Yamauchi, Daisuke Endo, Takeshi Hashimoto, Keiji Nagatani and 
   Ryo Kurazume, `Development of CPS Platform for Autonomous Construction <https://arxiv.org/pdf/2412.00147>`_, Arxiv, 2024

    .. code-block:: bibtex

       @inproceedings{kasahara2024arxiv,
         author    = {Yuichiro Kasahara, Kota Akinari, Tomoya Kouno, Noriko Sano, Taro Abe, Genki Yamauchi, Daisuke Endo, Takeshi Hashimoto, Keiji Nagatani and Ryo Kurazume},
         title     = {Development of CPS Platform for Autonomous Construction},
         booktitle = {Arxiv},
         year      = {2024},
       }   


4. Takayoshi Hachijo, Yutaro Fukase, Takashi Yokoshima, Yuki Miyashita, Shunsuke Kimura, Masanori Suzuki, Yuichiro Kasahara, Tomoya Kouno, 
   Koshi Shibata, Ryo Kurazume, Daisuke Endo, Genki Yamauchi, Takeshi Hashimoto, `3D Measurement System for Soil Loading by an Autonomous Backhoe using OPERA <https://www.iaarc.org/publications/2025_proceedings_of_the_42nd_isarc_montreal_canada/3d_measurement_system_for_soil_loading_by_an_autonomous_backhoe_using_opera.html>`_, 42nd International Symposium on Automation and Robotics in Construction (ISARC), 2025

    .. code-block:: bibtex

       @inproceedings{hachijo2025isarc,
         author    = {Takayoshi Hachijo, Yutaro Fukase, Takashi Yokoshima, Yuki Miyashita, Shunsuke Kimura, Masanori Suzuki, Yuichiro Kasahara, Tomoya Kouno, 
                      Koshi Shibata, Ryo Kurazume, Daisuke Endo, Genki Yamauchi, Takeshi Hashimoto},
         title     = {3D Measurement System for Soil Loading by an Autonomous Backhoe using OPERA},
         booktitle = {42nd International Symposium on Automation and Robotics in Construction (ISARC)},
         year      = {2025},
       }   

参考文献（国内会議）
--------------------------------------

1. 前田 龍一, 井塚 智也, 倉爪 亮, `土工現場用 CPS プラットフォーム ROS2-TMS for Construction の開発 <https://robotics.ait.kyushu-u.ac.jp/kurazume/papers/ROBOMECH23-4.pdf>`_, ロボティクス・メカトロニクス講演会(ROBOMECH), 2023

    .. code-block:: bibtex

       @inproceedings{maeda2023robomech,
         author    = {前田 龍一, 井塚 智也, 倉爪 亮},
         title     = {土工現場用 CPS プラットフォーム ROS2-TMS for Construction の開発},
         booktitle = {ロボティクス・メカトロニクス講演会(ROBOMECH)},
         year      = {2023},
       }      

2. 前田 龍一, 高野 智也, 松本 耕平, 中嶋 一斗, 倉爪 亮, `土工現場用CPS プラットフォームROS2-TMS for Construction の開発 -第2報360 度カメラ映像を用いたCPS 可視化実験- <https://robotics.ait.kyushu-u.ac.jp/kurazume/papers/SI23-1.pdf>`_, 第24回計測自動制御学会システムインテグレーション部門講演会(SI), 2023
  
    .. code-block:: bibtex

       @inproceedings{maeda2023si,
         author    = {前田 龍一, 高野 智也, 松本 耕平, 中嶋 一斗, 倉爪 亮},
         title     = {土工現場用 CPS プラットフォーム ROS2-TMS for Construction の開発},
         booktitle = {第24回計測自動制御学会システムインテグレーション部門講演会(SI)},
         year      = {2023},
       }      

3. 笠原 侑一郎, 井塚 智也, 柴田 航志, 前田 龍一, 高野 智也, 松本 耕平, 木村 駿介, 深瀬 勇太郎, 横島 喬, 山内 元貴, 遠藤 大輔, 橋本 毅, 倉爪 亮,
   `土工現場用 CPS プラットフォーム ROS2-TMS for Construction の開発- 第3報 タスク管理機構の実装- <https://robotics.ait.kyushu-u.ac.jp/kurazume/papers/ROBOMECH24-5.pdf>`_, ロボティクス・メカトロニクス講演会(ROBOMECH), 2024

    .. code-block:: bibtex

       @inproceedings{kasahara2024robomech,
         author    = {笠原 侑一郎, 井塚 智也, 柴田 航志, 前田 龍一, 高野 智也, 松本 耕平, 木村 駿介, 深瀬 勇太郎, 横島 喬, 山内 元貴, 遠藤 大輔, 橋本 毅, 倉爪 亮},
         title     = {土工現場用 CPS プラットフォーム ROS2-TMS for Construction の開発- 第3報 タスク管理機構の実装-},
         booktitle = {ロボティクス・メカトロニクス講演会(ROBOMECH)},
         year      = {2024},
       }      

4. 柴田 航志, 高野 智也, 笠原 侑一郎, 井塚 智也, 前田 龍一, 松本 耕平, 木村 駿介, 深瀬 勇太郎, 横島 喬, 山内 元貴, 遠藤 大輔, 橋本 毅, 倉爪 亮, 
   `土工現場用 CPS プラットフォーム ROS2-TMS for Construction の開発- 第4報 自律施工技術基盤 OPERA との連携- <https://robotics.ait.kyushu-u.ac.jp/kurazume/papers/ROBOMECH24-6.pdf>`_, ロボティクス・メカトロニクス講演会(ROBOMECH), 2024

    .. code-block:: bibtex

       @inproceedings{shibata2024robomech,
         author    = {柴田 航志, 高野 智也, 笠原 侑一郎, 井塚 智也, 前田 龍一, 松本 耕平, 木村 駿介, 深瀬 勇太郎, 横島 喬, 山内 元貴, 遠藤 大輔, 橋本 毅, 倉爪 亮},
         title     = {土工現場用 CPS プラットフォーム ROS2-TMS for Construction の開発- 第4報 自律施工技術基盤 OPERA との連携-},
         booktitle = {ロボティクス・メカトロニクス講演会(ROBOMECH)},
         year      = {2024},
       }      

5. 秋成 光太, 笠原 侑一郎, 高野 智也, 松本 耕平, 倉爪 亮, `土工現場用CPS プラットフォームROS2-TMS for Construction の開発 -第5報 没入感VRインターフェースOperaSimVRの開発- <https://robotics.ait.kyushu-u.ac.jp/kurazume/papers/SI24-2.pdf>`_,
   , 第25回計測自動制御学会システムインテグレーション部門講演会(SI), 2024

    .. code-block:: bibtex

       @inproceedings{akinari2024si,
         author    = {秋成 光太, 笠原 侑一郎, 高野 智也, 松本 耕平, 倉爪 亮},
         title     = {土工現場用CPS プラットフォームROS2-TMS for Construction の開発 -第5報 没入感VRインターフェースOperaSimVRの開発-},
         booktitle = {第25回計測自動制御学会システムインテグレーション部門講演会(SI)},
         year      = {2024},
       }      

6. 笠原 侑一郎, 高野 智也, 秋成 光太, 阿部 太郎, 山内 元貴, 遠藤 大輔, 橋本 毅, 永谷 圭司, 倉爪 亮,  `土工現場用 CPS プラットフォーム ROS2-TMS for Construction の開発 -第6報 自律施工技術基盤OPERAとの連携によるクローラダンプの制御- <https://robotics.ait.kyushu-u.ac.jp/kurazume/papers/ROBOMECH25-5.pdf>`_, ロボティクス・メカトロニクス講演会(ROBOMECH), 2025

    .. code-block:: bibtex

       @inproceedings{kasahara2025robomech1,
         author    = {笠原 侑一郎, 高野 智也, 秋成 光太, 阿部 太郎, 山内 元貴, 遠藤 大輔, 橋本 毅, 永谷 圭司, 倉爪 亮},
         title     = {土工現場用 CPS プラットフォーム ROS2-TMS for Construction の開発 -第6報 自律施工技術基盤OPERAとの連携によるクローラダンプの制御-},
         booktitle = {ロボティクス・メカトロニクス講演会(ROBOMECH)},
         year      = {2025},
       }      

7. 笠原 侑一郎, 高野 智也, 秋成 光太, 佐野 紀子, 八條 貴誉, 木村 駿介, 宮下 裕貴, 深瀬 勇太郎, 横島 喬, 阿部 太郎, 山内 元貴, 遠藤 大輔, 橋本 毅, 永谷 圭司, 倉爪 亮,
   `土工現場用 CPS プラットフォーム ROS2-TMS for Construction の開発 -第7報 バックホウ・クローラダンプと複数 3D LiDAR による土砂の掘削・積載・運搬・放土作業の自動化- <https://robotics.ait.kyushu-u.ac.jp/kurazume/papers/ROBOMECH25-1.pdf>`_, 2025
   
    .. code-block:: bibtex

       @inproceedings{kasahara2025robomech2,
         author    = {笠原 侑一郎, 高野 智也, 秋成 光太, 佐野 紀子, 八條 貴誉, 木村 駿介, 宮下 裕貴, 深瀬 勇太郎, 横島 喬, 阿部 太郎, 山内 元貴, 遠藤 大輔, 橋本 毅, 永谷 圭司, 倉爪 亮},
         title     = {土工現場用 CPS プラットフォーム ROS2-TMS for Construction の開発 -第7報 バックホウ・クローラダンプと複数 3D LiDAR による土砂の掘削・積載・運搬・放土作業の自動化-},
         booktitle = {ロボティクス・メカトロニクス講演会(ROBOMECH)},
         year      = {2025},
       }     

8.  秋成 光太, 笠原 侑一郎, 高野 智也, 佐野 紀子, 松本 耕平, 山内 元貴, 遠藤 大輔, 阿部 太郎, 橋本 毅, 永谷 圭司, 倉爪 亮, `土工現場用 CPS プラットフォーム ROS2-TMS for Construction の開発 -第8報 没入感VRインターフェース OperaSimVR を用いたPreviewed Reality実験- <https://robotics.ait.kyushu-u.ac.jp/kurazume/papers/ROBOMECH25-4.pdf>`_, ロボティクス・メカトロニクス講演会, 2025

    .. code-block:: bibtex

        @inproceedings{akinari2025si,
          author    = {秋成 光太, 笠原 侑一郎, 高野 智也, 佐野 紀子, 松本 耕平, 山内 元貴, 遠藤 大輔, 阿部 太郎, 橋本 毅, 永谷 圭司, 倉爪 亮},
          title     = {土工現場用 CPS プラットフォーム ROS2-TMS for Construction の開発 -第8報 没入感VRインターフェース OperaSimVR を用いたPreviewed Reality実験-},
          booktitle = {ロボティクス・メカトロニクス講演会(ROBOMECH)},
          year      = {2025},
        }     


報道発表
--------------------------------------

1. 日刊工業新聞, `油圧ショベル自動化 清水建など実証 <https://www.nikkan.co.jp/articles/view/00751041>`_, 2025.6.6
2. 日刊建設工業新聞,  `清水建設ら/油圧ショベルの掘削〜積載をシームレスに自動化、土研の技術基盤活用 <https://nikkankensetsukogyo2.blogspot.com/2025/06/blog-post_88.html>`_, 2025.6.6
3. 建設通信新聞, 土砂掘削・積載を自動化/ 「OPERA」の有効性確認, 2025.6.6
4. 清水建設 ニュースリリース,   `油圧ショベルによる土砂掘削・ダンプ積載作業を自動化 ～土木研究所の自動施工技術基盤「OPERA」の有効性を確認～ <https://www.shimz.co.jp/company/about/news-release/2025/2025011.html>`_, 2025.6.6


Contents
--------

.. toctree::

   Install
   TaskManagementMechanism
   DataBase
   CustomNodes
   Sensing
   Tutorial
   Blackboards

