## AWS Cloud 상에서 Java 개발환경 설정

### 개인적인 동기들
 몇년 만에 개발 공부를 다시 시작하기로 마음 먹고, 혼자 공부를 시작하고, 여러 부트캠프들을 알아보던 중에 가장 큰 고민거리는 다름아닌 개발 환경이었다. 기존에 이력서 작성 및 웹서핑을 위해 완전 후진 노트북을 하나 가지고 있었는데, Windows 10은 물론이고 Linux로 전환하고 Ubuntu Gnome을 돌리는데도 부족한 사양이여서 인내심을 가지고 사용하고 있었다. <br>
 여건상 새로운 노트북을 구매하긴 힘들었고, 기존에 보유 중이던 iPadPro 11형 3세대를 좀 더 활용하고자 클라우드 환경을 알아보았다. 이 때는 어떤 부트캠프를 참여할지도 고민 중이었고, BackEnd FrontEnd 진로도 확실하게 결정하기 전이라 그나마 관심있었던 python 개발 환경 구축을 위해 최선을 다했는데, 클라우드 프리티어는 어쩔 수 없이 CLI (Command Line Interface)에 익숙해져야 했고, 이 환경에서 개발 공부 하려면 Vim은 그냥 좋든 싫든 필수적으로 익혀야 했다... 배보다 배꼽이 더 커진 느낌이었지만 지금 와서 돌아보니 나름 Vim을 사용하는 것도 조금 적응이 되었고, 만고의 삽질 끝에 python 개발은 물론 Java 개발환경까지 구축했다. 게다가 현재는 제로베이스 백엔드 스쿨까지 참여 중이니 여러모로 도움이 되는 삽질이였다!<br>
 <u>Java 공부에 Data Structure, Algorithm 도 본격적으로 공부를 시작해야 하는 판국에 Start가 이상한 것 같다는 느낌도 받았지만,(부끄러우니까 취소선...)</u> 일단 첫번째로 <strong>삽질이 재미가 있었다.</strong> 개발 블로그도 나름 시작해보겠다고 찾아보다가 네이버 벨로그 이런데 글쓰기 보다 뭔가 Vim을 활용하고 싶다는 생각에 꽂혀서 깃헙 페이지부터 알아보기 시작하고 ... Java 문법도 잘 모르면서 Jekyll 테마 적용해보겠다고 또 삽질하고 ... 시간을 효율적으로 쓰진 못했지만 (아는거 하나 없이 혼자서 삽질부터 했으니 당연한 결과였다.)<br>
 욕심은 언젠가 shell script까지 섭렵해서 아래 서술되는 환경 설정도 다 자동화 해버리는 것이다. 이미 이를 수행한 여러 능력자분들 [Christian Chiarulli](https://github.com/ChristianChiarulli), [EdenEast](https://github.com/EdenEast) 이 발판을 마련해주셨기 때문에 시간만 많이 투자하면 충분히 가능하다! 상상만해도 너무 짜릿하다. Instance 생성해서 아이패드로 접속한 후에 wget curl 둘 중 아무거나 이용해서 내 깃헙 주소 참조해서 명령어 하나만 수행하면 [oh-my-zsh](https://ohmyz.sh) [SdkMan](https://sdkman.io) 처럼 녹색 글자 주르르르륵 나오면서 자동으로 zsh 설정되는 것을 시작으로 나만의 개발환경이 구축된다. 이런거 잘할려고 자료구조 공부하고, 컴퓨터 구조 공부하고, 알고리즘 공부하고 그런거 아닐까 ! 아무튼 잡설 스톱하고 얼른 README 작성 마무리해야지.
 

### 기본환경
 Ubuntu 22.04 - AWS EC2 <br>
 Instance 생성시 SSH Key 생성이 필수.

### 설정 순서
#### 1. Swapfile 설정
```console
user@hostname ~$  sudo swapoff -a
user@hostname ~$  sudo dd if=/dev/zero of=/swapfile bs=1M count=2000
user@hostname ~$  sudo mkswap /swapfile
user@hostname ~$  sudo chmod 0600 /swapfile
user@hostname ~$  sudo swapon /swapfile
user@hostname ~$  
```
 'YouCompleteMe' 플러그인 설정 중에 메모리 부족으로 인한 Compile 중단 현상이 발생한 적이 있었다. <br/>
 해당 문제를 해결하기 위해 [c++: internal compiler error (메모리 에러)](https://m.blog.naver.com/jungspeedy/222036268371) 내용을 참고하여 설정하였더니 더이상 중단되지 않았다. <br/>
 대부분의 패키지들은 apt를 통해 관리가 가능하지만 간혹 Source를 직접 빌드해야 하는 경우가 발생하는데, 메모리 문제를 사전에 방지할 수 있어서 가장 먼저 설정한다. <br/>


#### 2. Neovim 설치
 [Neovim PPA team](https://launchpad.net/~neovim-ppa/+archive/ubuntu/stable) 내용을 참고하여 설치한다. <br>
```console
user@hostname ~$  sudo add-apt-repository ppa:neovim-ppa/stable 
user@hostname ~$  sudo apt update
user@hostname ~$  sudo apt install neovim
user@hostname ~$  nvim
```
 기본 내장된 vi/vim 보다 향상된 기능을 지원하는데 lua language를 통해 Plugin 관리 뿐만 아니라 설정, 확장 등이 용이하다. <br>
 Neovim을 직접적으로 설정할 필요는 없다. Neovim 기반의 LunarVim 을 설치하면 기본 환경 설정은 완료되어 있으며, 추가적으로 몇가지 환경 설정을 하면 Java 개발을 위한 환경을 구축할 수 있다. LunarVim은 Neovim 0.7.0 이상의 버전을 필요로 한다. apt-repository 추가없이 neovim을 설치할 경우 0.5.0 버젼이 설치되므로 0.7.0 버젼 이상을 설치하기 위해 반드시 apt-repository를 추가한다.<br> 
 Neovim 실행 후 ':versions', ':checkhealth' 명령을 통해 버전 확인 및 기본 환경 체크가 가능하다. 기본적인 명령어는 vim과 동일하므로 관련 자료들을 참고한다.


#### 3. 기본 패키지 한번에 설치
pyenv, tmux-mem-cpu-load, lazygit, git-graph, sdkman 등의 설치를 위해 필요한 package들을 한 번에 설치한다.
```console
user@hostname ~$ sudo apt-get install -y make build-essential libssl-dev zlib1g-dev libbz2-dev \
libreadline-dev libsqlite3-dev wget curl llvm libncurses5-dev libncursesw5-dev \
xz-utils tk-dev zip unzip cargo cmake 

```

#### 3. tmux-mem-cpu-load 설치 및 ~/.tmux.conf 파일 설정
 iPad 에서 Termius App을 이용하여 AWS EC2 Incantance SSH 에 접속할 경우 화면이 작고 GUI 사용이 불가능하다. 이 때 tmux를 이용하면 Command Line Interface 에서도 Multi-tasking이 가능하다. <br>
[tmux 사용 이미지 삽입]

##### [tmux-mem-cpu-load](https://github.com/thewtex/tmux-mem-cpu-load) - CPU, RAM, and load monitor for use with tmux
 Tmux에서도 Linux GUI 환경이나 Windows의 상태표시줄과 같은 기능을 하는 Status Bar가 표시된다. 이 때 상태표시줄에서 현재 Memory 사용량 등을 확인할 수 있게 해주는 프로그램이다. AWS Instance Free Tier의 경우 가용 RAM의 크기가 매우 작기 때문에 심심하면 먹통이 되는 경우가 자주 발생한다. <br>
 Memory 사용량이 실시간으로 변할때 대응하기도 전에 먹통이 되기 때문에 불필요하다고 느껴질 수도 있지만 최소한 어느 순간에 사용량이 급격히 늘어나는지 파악할 수 있긴 하다. 자세히 찾아보진 않았지만 대략 한달 3~6만원 정도로 15GB 정도의 RAM을 이용할 수 있다. 조금 고민되지만 과금 폭탄이 두렵기 때문에 확실히 알아보고 추후에 신청을 해보려고 한다. 그때 이 프로그램을 좀 더 유용하게 쓸 수 있지 않을까 한다.


 ```
  set-option -sg escape-time 10
  set-option -g focus-events on
  set-option -g default-terminal "xterm-256color"
  set-option -sa terminal-overrides ',xterm-256color:RGB'
 ```

#### 4. pyenv & virtualenv 를 통한 python virtual environment 설정
 Java 개발을 위한 용도지만 마찬가지로 neovim plugin의 원활한 활용을 위해 이를 설정한다. [ubuntu에서 pyenv 설치하기](https://jinmay.github.io/2019/03/16/linux/ubuntu-install-pyenv-1/)
 'pyenv global' 명령을 통해 python 설정이 완료되면 pip install pynvim neovim 명령을 통해 해당 패키지들을 설치한다.

#### 5. nodejs & npm 설치
 기본 IDE로 사용할 LunarVim은 mason plugin을 통해 여러 LSP를 편리하게 설정할 수 있다. <br> 
 이 때 설치되는 Language Server 들이 nodejs를 통해 구현되어있기 때문에 필수적으로 환경 설정이 필요하다. <br>
 [NodeSource Node.js Binary Distributions](https://github.com/nodesource/distributions/blob/master/README.md#debinstall) 내용을 참고하여 LTS 버젼을 설치한다.

#### 6. cargo 설치
  ```
  sudo apt install cargo
  ```
  rust build system & package manager -> 필수는 아니지만 Lunarvim 설치시에 환경 설정이 나옴. ( 자세한 내용은 잘 모르겠다. 아직 Java 배우기에 바빠서 Rust 까지는 무리..)

#### 7. SDKMAN 설치. -> Java 개발을 위해 가장 중요.
(작성중)
