EC2（Amazon Elastic Compute Cloud）

オンプレミスでサーバーを構築する場合
1. どのくらいの規模が必要か調査してから購入する
2. ベンダーからサーバーが届くまでに、数週間～数か月がかかる。
3. サーバーをラックへ設置、配線し、セキュリティを確保、電源が入っているかの確認をして、やっと使用準備が整う。

	：一番の問題点は、サーバーを使っても使わなくても維持しなければならないこと。


EC2は、既にデータセンターの構築とセキュリティ対策まで済んでおり、いつでもオンラインで使用でき、常に膨大な規模のコンピュータを運用しているので、EC2インスタンス※1をリクエストするだけで必要な時に必要なだけ使用できる（数分以内に準備完了）。

※1インスタンス = ユーザーごとに切り分けたコンピュータ


使用料金は従量課金制で、停止中・終了後は発生しない。


EC2インスタンスを作成する場合、ほかの複数のインスタンスとホストを共有（これは仮想マシンとも呼ばれる）。

ホストで実行するハイパーバイザーは物理リソースを共有する役割があり、それをマルチテナンシーと呼ぶ。ハイパーバイザーにはマルチテナンシーを調整する役割がある。

リソースを共有しているが、インスタンス同士は互いの存在を認識していない。


EC2インスタンスをプロビショニングする場合、Windows または Linux を選択でき、そのうえでソフトウェアも扱える。

プロビジョニングとは、**必要に応じてネットワークやコンピュータの設備などのリソースを提供できるよう予測し、準備しておくこと**

Amazon EC2の仕組み
⭐1．インスタンスを起動
	基本設定を含むテンプレートを選択。この基本設定には、OS、アプリケーションサーバー（プログラミング言語ごとに選ぶ）、アプリケーション（OS、サーバー、言語環境、CMS※2）などが含まれる。また、インスタンスタイプ※3も設定。

※2 CMS = Content Management System、WordPressなどのWebサイトを作れるツール
※3 インスタンスタイプ = 目的に応じて選ぶスペック。
	t系（バースト（一瞬だけ性能を引き上げる）可能、低コスト）：軽いWebサイト・テスト
 	m系（汎用）　：バランスが良い
  	c系（計算重視）：データ処理・APIサーバーなど
   	r系（メモリ重視）：大きなデータ処理・DB向け
    g,p,inf系（GPU）：機械学習、画像処理

⭐2.　接続
	インスタンスに接続。
		1.　SSH（Secure Shell） 接続※4←最も一般的
			（Linux インスタンス）
		2.　RDP（リモートデスクトップ接続）←Windowsインスタンスの場合
		3.　ブラウザやHTTPリクエスト※5経由での接続
		4.　AWS Systems Manager Se4ssion Manager(SSM)　
			EC2インスタンスに事前設定すれば、SSHやRDP不要で接続できるセキュアな方法。ブラウザ上のマネジメントコンソールから直接コマンドを実行できる。
		5.　プログラムや他のAWSサービスからの接続
			別のEC2やLambda※6、S3※7、RDS※8からアクセスする
※4 SSH接続 = リモートマシンと安全に通信するためのプロトコル（通信のルールで、順番、形式、返答方法が詳細に決まっている）で、EC2インスタンスにログインしてコマンド操作をする際に使用する
※5 HTTP（Hypertext Transfer Protocol）は、Webブラウザとサーバー間で情報をやり取りする通信規約
※6 Lambda = 必要なときだけ自動で実行できる機能（EC2はアクセスが0でも、手動で停止しない限りは常時起動している）
※7 S3(Simple Storage Service) = オブジェクト（ファイル）を保存するためのストレージサービスで、ECやLambdaからのアップロード、ダウンロードが可能
※8 Amazon RDS(Relational Database Service) = SQL※9 に対応しており、自動バックアップ、スケーリング（処理能力、分散処理能力の増加）、メンテナンス（セキュリティ・バグ修正・自動復旧など）が行われる。EC2のアプリケーションから接続可能
※9 SQL（Structured Query Language）= 構造化問い合わせ言語、データベースを参照するための言語で、追加・更新・削除も可能。