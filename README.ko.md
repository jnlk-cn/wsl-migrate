<h1 align="center">WSL2 배포판 마이그레이션</h1>

<p align="center">
  <a href="https://github.com/jnlk-cn/wsl-migrate/stargazers"><img src="https://img.shields.io/github/stars/jnlk-cn/wsl-migrate?style=flat-square" alt="GitHub stars"></a>
  <a href="https://github.com/jnlk-cn/wsl-migrate/network/members"><img src="https://img.shields.io/github/forks/jnlk-cn/wsl-migrate?style=flat-square" alt="GitHub forks"></a>
  <a href="https://github.com/jnlk-cn/wsl-migrate/issues"><img src="https://img.shields.io/github/issues/jnlk-cn/wsl-migrate?style=flat-square" alt="GitHub issues"></a>
  <a href="LICENSE"><img src="https://img.shields.io/github/license/jnlk-cn/wsl-migrate?style=flat-square" alt="License"></a>
  <a href="https://github.com/jnlk-cn/wsl-migrate/commits/main"><img src="https://img.shields.io/github/last-commit/jnlk-cn/wsl-migrate?style=flat-square" alt="Last commit"></a>
</p>

<p align="center">
  <a href="README.md">English</a> |
  <a href="README.zh-CN.md">简体中文</a> |
  <a href="README.ja.md">日本語</a> |
  <a href="README.ko.md">한국어</a>
</p>

---

WSL2 배포판을 시스템 드라이브에서 다른 드라이브로 마이그레이션하기 위한 간결한 가이드와 Skill입니다.

## 왜 마이그레이션해야 할까요?

WSL2 배포판은 사용 시간이 늘어남에 따라 수십 GB의 공간을 차지할 수 있습니다. 시스템 드라이브(보통 `C:`)에서 이동하면 공간을 확보하고 백업도 쉬워집니다.

## 빠른 시작

### 1. 현재 배포판 확인

```powershell
wsl -l -v
```

### 2. 내보내기 및 가져오기

```powershell
# WSL 종료
wsl --shutdown

# 내보내기
wsl --export Ubuntu-24.04 D:\WSL\ubuntu.tar

# 새 위치로 가져오기
wsl --import ubuntu D:\WSL\Ubuntu D:\WSL\ubuntu.tar
```

### 3. 기본 사용자 복원

가져오기 후 WSL은 기본적으로 `root`로 로그인됩니다. `/etc/wsl.conf`를 생성하여 수정하세요:

```powershell
wsl -d ubuntu -u root -e bash -c "tee /etc/wsl.conf << 'EOF'
[user]
default=<당신의Linux사용자명>
EOF"
wsl --terminate ubuntu
```

### 4. 이전 배포판 정리

정상 작동을 확인한 후 이전 배포판을 등록 취소하고 tar 파일을 삭제합니다:

```powershell
wsl --unregister Ubuntu-24.04
Remove-Item D:\WSL\ubuntu.tar
```

## 전체 가이드

다중 배포판 마이그레이션, 배포판 이름 변경, 안전 팁 등을 포함한 완전한 단계별 워크플로우는 [`SKILL.md`](SKILL.md)를 참조하세요.

## Star 기록

[![Star History Chart](https://api.star-history.com/svg?repos=jnlk-cn/wsl-migrate&type=Date)](https://star-history.com/#jnlk-cn/wsl-migrate&Date)

## 라이선스

이 프로젝트는 [MIT 라이선스](LICENSE) 하에 라이선스됩니다.
