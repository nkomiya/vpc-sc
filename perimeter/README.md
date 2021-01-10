# perimeter

**目次**

<!-- TOC -->

- [perimeter](#perimeter)
    - [サービス境界の作成](#サービス境界の作成)
    - [サービス境界の編集](#サービス境界の編集)
        - [許容する API の追加](#許容する-api-の追加)
        - [許容する API の削除](#許容する-api-の削除)
        - [API 呼び出しを全面的に禁止する](#api-呼び出しを全面的に禁止する)

<!-- /TOC -->

## サービス境界の作成

```bash
# GCP project ID
$ PROJECT_ID=...
# GCP 組織名
$ ORGANIZATION_NAME=...
# サービス境界名
$ PERIMETER_NAME=...
# 保護対象 API (カンマ区切り)
$ RESTRICTED=...
# Private Google アクセス (GCE 等) から許容する API (RESTRICTED-SERVICES で、保護対象 API のみの呼び出しに制限する)
$ ALLOWED=RESTRICTED-SERVICES

# サービス境界作成
$ gcloud access-context-manager perimeters create ${PERIMETER_NAME} \
    --title=${PERIMETER_NAME} \
    --resources=projects/$(gcloud projects describe ${PROJECT_ID} --format 'value(projectNumber)') \
    --restricted-services=${RESTRICTED} \
    --enable-vpc-accessible-services --vpc-allowed-services=${ALLOWED}
```

## サービス境界の編集

Private Google アクセスから呼び出せる API を変更する方法について。

### 許容する API の追加

```bash
# 許容する API (カンマ区切り)
$ ALLOWED=...

# 許容する API の追加
$ gcloud access-context-manager perimeters update ${PERIMETER_NAME} \
    --enable-vpc-accessible-services --add-vpc-allowed-services=${ALLOWED}
```

### 許容する API の削除

RESTRICTED-SERVICES に含まれる特定のサービスを除くことはできないことに注意。

```bash
# 許容する API (カンマ区切り)
$ DISALLOW=...

# 許容する API の削除
$ gcloud access-context-manager perimeters update ${PERIMETER_NAME} \
    --remove-vpc-allowed-services=${DISALLOW}
```

### API 呼び出しを全面的に禁止する

```bash
$ gcloud access-context-manager perimeters update ${PERIMETER_NAME} \
    --clear-vpc-allowed-services
```
