# 필요한 패키지 (ruby, jekyll, node) 설치
세가지 패키지를 [github pages versions](https://pages.github.com/versions/) 링크의 패키지 버전에 맞게 설치

## ruby
```bash
brew install rbenv ruby-build
```
- ~/.zshrc 에 아래 내용추가

`vim ~/.zshrc`
```vim
eval "$(rbenv init -)"
```
`source ~/.zshrc`

- 특정 버전 ruby 설치
```bash
rbenv install 3.3.4
```

- 버전 설정
```bash
rbenv global 3.1.2
```
`ruby -v` 로 버전 확인. 만약 변경이 되어있지 않다면 `source ~/.zshrc` 명령어 수행 후 확인하기

## jekyll
```bash
gem install jekyll bundler
```

## node
```bash
brew install node
```

# 링크를 통해 jekyll 테마 포크
[chirpy](https://github.com/cotes2020/jekyll-theme-chirpy/fork)
- 이 떄 포크하는 레포의 이름은 반드시 {github_id}.github.io 여야함.

    ex) sweaty-fingers.github.io

1. branch branch 이름을 master -> main 으로 변경 (setting/general 에서 변경가능) 
2. setting/branches -> branch protection rule 에서 `branch name pattern`을 main 으로 바꾸고 protect matching branches에서 아래 두 항목의 체크 해제 확인
    - Allow force pushes
    - Allow deletions

3. 로컬로 clone
```bash
git clone {레포 주소}
```

4. 필요 모듈 설치
- 로컬 레포 디렉터리로 가서 아래 명령어 수행
```bash
bundle
```
5. npm 을 통해 node.js 모듈 설치
```bash
npm install && npm run build
```

6. 아래 명령어로 로컬에서 jekyll 실행 확인
```bash
jekyll serve
```

# github 배포 설정
1. settings/pages -> build and deployment/source를 github actions로 변경.
2. configure를 선택 후 별도의 수정 없이 commit changes
    - .github/workflows 에 pages-deploy.yml.hook 파일이 존재할 경우 삭제.
3. 원격 브랜치의 내용을 로컬에 가져오기
```bash
git pull
```

4. .gitignore 에서 assets/js/dist 내 파일들의 push를 무시하는 설정 주석처리
```vim
# Bundler cache
.bundle
vendor
Gemfile.lock

# Jekyll cache
.jekyll-cache
.jekyll-metadata
_site

# RubyGems
*.gem

# NPM dependencies
node_modules
package-lock.json

# IDE configurations
.idea
.vscode/*
!.vscode/settings.json
!.vscode/extensions.json
!.vscode/tasks.json

# Misc
# _sass/dist # 주석처리?
# assets/js/dist # 주석처리

```

5. push 후 블로그 확인
- push 할 때 아래와 같은 에러가 발생할 수 있음. commit 시 특정 메세지 포맷을 지키도록 강요하는 내용. 
```bash
git commit -m "test"
p⧗   input: test
✖   subject may not be empty [subject-empty]
✖   type may not be empty [type-empty]

✖   found 2 problems, 0 warnings
ⓘ   Get help: https://github.com/conventional-changelog/commitlint/#what-is-commitlint
```

- 아래와 같은 형식으로 커밋 메세지 작성
```bash
<type>(<scope>): <subject>
```
ex)
```bash
git commit -m "feat(core): add new feature"
```