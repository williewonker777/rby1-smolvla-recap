# RB-Y1 RECAP 공개 평가 명령

패치를 적용한 수업 저장소와 공식 실습 환경을 먼저 준비한다. 별도 제공받은 R1 배포 폴더를 `POLICY_PATH`로 지정한다. 이 코드 저장소에는 가중치를 포함하지 않는다.

```bash
COURSE=/absolute/path/to/patched/lightwheel_rby1_transfer
POLICY_PATH=/absolute/path/to/R1/pretrained_model
OUTPUT=/absolute/path/to/new/r1-eval-output
test -s "$POLICY_PATH/model.safetensors" && test ! -e "$OUTPUT" || exit 1
cd "$COURSE" || exit 1
./vla_lab/eval_smolvla.sh --policy-path "$POLICY_PATH" --output "$OUTPUT" --device cuda:0
```

기본 설정으로 T1–T10, 과업당 공개 seed 50개, 50-action chunk를 사용한다. 평가 코드 및 환경 설정은 원본과 같다. R1에는 저장된 양성 processor가 필요하고, 가치 모델과 훈련 라벨·optimizer 상태는 필요 없다.
