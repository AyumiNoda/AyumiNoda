# 見込み作成時作業マニュアル

sequenceDiagram
	autonumber

	actor user as ユーザー
	actor staff as スタッフ
	participant reception_system as チェックインカウンター
	participant matrix_api as アトラクションAPI
	participant onp_iapi as ONP-IAPI

	user ->> user: リサイクル契機の体験
	user ->> staff: 報告
	staff ->> staff: 状況確認
	staff ->> user: リサイクル対象であることの連絡
	staff ->> reception_system: リサイクルの処理（3人一括）
	reception_system ->> matrix_api: リサイクルの処理
	matrix_api ->> matrix_api: リサイクルの処理
	Note over matrix_api: 古いGmaeSessionをendedに設定<br>古いPlayerのrfidを解除しendedに設定<br>新しいGameSessionをstandbyで作成<br>GameSessionQueueに新しいGameSessionを登録<br>Playerをstandbyにし、rfidを設定
	matrix_api ->> onp_iapi: POST /parties/recycle
	Note over matrix_api,onp_iapi: 変更されたデバイス情報をhhdeviceに含める
	onp_iapi -->> matrix_api: ok
	matrix_api ->> reception_system: 新しいエントリーナンバー
	reception_system ->> staff: 新しいエントリーナンバー
	staff ->> user: 新しいエントリーナンバーの掲示
