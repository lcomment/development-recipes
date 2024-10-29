# SNS 시스템 규모 추정

## 가정

- `300,000,000` `monthly` active users
- `50%` of users write `daily`
- Users post `2 postings per day` on average
- `10%` of postings contain `media`
- Data is stored for `5 years`

### Post 사이즈

- post_id: 64 bytes
- text: 140 bytes
- media: 1 MB

## 추정

- 활성 유저
  - 3억
- MAU
  - 3억
- DAU
  - 3억 x 0.5
  - `1.5억`
- 하루에 작성되는 게시글
  - 1.5억 x 2
  - `3억`
- 하루에 쌓이는 미디어 데이터 용량
  - 3억 x 0.1 x 1MB
  - `30,000,000 MB`
  - 30TB
- 5년간 쌓이는 미디어 데이터 용량
  - 30TB x 365 x 5
  - `54,750TB`
  - 54.75PB
- QPS
  - 3억 / (24 x 3600)
  - `3,472`
- Peek QPS
  - 3,472 x 2 = 6,944
  - 3,472 x 4 = 13,888
