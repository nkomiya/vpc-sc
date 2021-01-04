# vpc-sc

<!-- TOC -->

- [vpc-sc](#vpc-sc)
    - [共通設定](#共通設定)
        - [組織ポリシーの設定](#組織ポリシーの設定)

<!-- /TOC -->

## 共通設定

VPC Service Controls を利用する際、共通して必要となる作業について記載する。

### 組織ポリシーの設定

- 組織IDの取得

```bash
# 変数設定
$ ORGANIZATION_NAME={{組織名}}

# ID取得
$ ORGANIZATION_ID=$(\
    gcloud organizations list \
        --filter "displayName = ${ORGANIZATION_NAME}" \
        --format 'value(name)') &&
  echo ${ORGANIZATION_ID}
```

- 組織ポリシーの作成

```bash
# 変数設定
POLICY_TITLE={{ポリシー名}}

# ポリシー作成
gcloud access-context-manager policies create \
    --organization ${ORGANIZATION_ID} \
    --title "${POLICY_TITLE}"
```

- gcloud コマンドの設定

```bash
# 変数設定
$ POLICY_NAME=$(\
    gcloud access-context-manager policies list \
        --organization ${ORGANIZATION_ID} \
        --format 'value(name)')

# ACM のデフォルトポリシーを指定する
$ gcloud config set access_context_manager/policy ${POLICY_NAME}
```
