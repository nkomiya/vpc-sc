# access-level

**目次**

<!-- TOC -->

- [access-level](#access-level)
    - [概要](#概要)
    - [サンプル](#サンプル)
        - [アクセスレベルの作成](#アクセスレベルの作成)
            - [IP アドレスの許容](#ip-アドレスの許容)
            - [User Account の許容](#user-account-の許容)
        - [アクセスレベルの割当](#アクセスレベルの割当)

<!-- /TOC -->

## 概要

Access Level をサービス境界に設定し、許容するアクセスを定義する。

## サンプル

### アクセスレベルの作成

#### IP アドレスの許容

```bash
# アクセスを許容する IP アドレス
$ IP_ADDRESS=...

$ gcloud access-context-manager levels create AllowedAddresses \
    --title 'Allow access from specific IP addresses' \
    --basic-level-spec <(IP_ADDRESS=${IP_ADDRESS} envsubst <$(git rev-parse --show-toplevel)/access-level/ip.yaml) \
    --combine-function=OR
```

#### User Account の許容

```bash
# アクセスを許容するユーザアカウントのアドレス
$ EMAIL=...

$ gcloud access-context-manager levels create AllowedUsers \
    --title 'Allow access by user accounts' \
    --basic-level-spec <(EMAIL=${EMAIL} envsubst <$(git rev-parse --show-toplevel)/access-level/user.yaml) \
    --combine-function=OR
```

### アクセスレベルの割当

```bash
# サービス境界
$ PERIMETER=...
# アクセスレベルレベル名 (カンマ区切り)
$ LEVELS=...

$ gcloud access-context-manager perimeters update ${PERIMETER} \
    --add-access-levels ${LEVELS}
```
