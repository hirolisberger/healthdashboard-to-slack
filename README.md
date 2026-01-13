# healthdashboard-to-slack

AWS Healthのイベント通知をSlackに送信するための設定を提供します。
AWS ChatbotとSNSトピックを利用して、AWSのステータスをリアルタイムで監視し、イベントをSlackチャネルに通知します。

## プロジェクト構成

- **template.yaml**: AWSリソースを定義するCloudFormationテンプレート。
  - **Parameters**
    - **SlackChannelId**: 通知先のSlackチャネルID。
    - **SlackWorkspaceId**: 通知先のSlackワークスペースID。
  - **Resources**
    - **HealthCheckChatbot**: AWS Chatbot設定。Slackの通知設定に必要なチャンネル情報とロールを設定。
    - **HealthCheckTopic**: 通知メッセージの送信先として使用されるSNSトピック。
    - **HealthCheckTopicPolicy**: SNSトピックに対するEventBridgeの許可ポリシー。
    - **HealthCheckTopicSubscription**: SNSトピックとAWS Chatbotのエンドポイントのサブスクリプション。
    - **HealthCheckEventRule**: EventBridgeルールで、`AWS Health Event` をトリガーとしてSNSトピックに通知を送信。

- **samconfig.toml**: AWS SAMのデプロイ設定ファイル。
  - **stack_name**: スタック名。
  - **region**: デプロイ先のリージョン。
  - **s3_bucket**: SAMの一時ファイル保存に使用するS3バケット。
  - **capabilities**: IAMリソース作成の許可（`CAPABILITY_NAMED_IAM`）。


## 事前準備

- **AWS CLI** および **AWS SAM CLI** をインストールし、設定していること。
- AWS ChatbotのSlack設定に必要なSlackワークスペースIDとチャネルIDを取得していること。
- **S3バケット**が作成されていること（`samconfig.toml` に記載）。

## デプロイ手順

sam deploy --config-file samconfig.toml \
    --parameter-overrides SlackChannelId="********" SlackWorkspaceId="********"
