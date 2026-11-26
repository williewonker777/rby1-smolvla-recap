# RB-Y1 SmolVLA에 RECAP 이식

원본과 같은 RB-Y1 T1–T10 환경에서, 같은 자율 경험으로 학습한 행동모방(B1)과 RECAP(R1)을 비교한다. 구현은 π*0.6 RECAP의 가치 추정 및 advantage 조건화에 기반한다. **공개 평가는 진행 중이며 성공률 향상 여부는 아직 확정되지 않았다.**

## 설치와 재현

수업의 `lightwheel_rby1_transfer`의 원본 커밋 `6e869d5c95341c740e38fa8bebcc658bfaa6c64f` 및 실습 환경이 필요하다. 이 저장소에는 추가한 코드·테스트만 패치로 제공한다. 수업 저장소의 장면, 카메라, 로봇, 성공 판정, 평가 horizon을 바꾸지 않는다.

```bash
COURSE=/absolute/path/to/lightwheel_rby1_transfer
BUNDLE=/absolute/path/to/this/repository
test "$(git -C "$COURSE" rev-parse HEAD)" = 6e869d5c95341c740e38fa8bebcc658bfaa6c64f || exit 1
git -C "$COURSE" apply --check --whitespace=error-all "$BUNDLE/RECAP-course.patch" &&
git -C "$COURSE" apply --whitespace=error-all "$BUNDLE/RECAP-course.patch" &&
(cd "$COURSE" && sha256sum -c "$BUNDLE/PORT-CODE.sha256")
```

적용 후 수업 저장소의 `vla_lab/RECAP.md`대로 오라클 시범 수집 → B0 학습·평가·진단 → 실제 정책 롤아웃 수집 → 가치 함수·advantage 라벨 생성 → B1/R1 동일 예산 학습 → 공식 평가 순서로 재현한다. [STUDY-CONFIG.json](STUDY-CONFIG.json)은 실행 설정의 기록이며 수업 스크립트를 대신하지 않는다. [평가 명령](EVAL-COMMAND.md)은 `pretrained_model`을 별도로 확보해 실행한다.

깨끗한 원본에 패치를 재적용해 파일 24개 해시가 일치했고, 실제 T1 스모크를 포함해 테스트 114개와 하위 테스트 89개를 통과했다. 학습 데이터, 동영상, 가중치, 환경 캐시 및 로그는 이 코드 공개본에 포함하지 않는다.
