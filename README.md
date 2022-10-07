# AWS Cloud 상에서 Java 개발환경 설정

### 기본환경
 Ubuntu 22.04 - AWS EC2 <br>
 Instance 생성시 SSH Key 생성이 필수.

### 설정 순서
#### 1. Swapfile 설정
 'YouCompleteMe' 플러그인 설정 중에 메모리 부족으로 인한 Compile 중단 현상이 발생한 적이 있었다. <br>
 해당 문제를 해결하기 위해 [c++: internal compiler error (메모리 에러)](https://m.blog.naver.com/jungspeedy/222036268371) 내용을 참고하여 설정하였더니 더이상 중단되지 않았다. <br>
 대부분의 패키지들은 apt를 통해 관리가 가능하지만 간혹 Source를 직접 빌드해야 하는 경우가 발생하는데, 메모리 문제를 사전에 방지할 수 있어서 가장 먼저 설정한다. <br>

#### 2. Neovim 설치
 [Neovim PPA team](https://launchpad.net/~neovim-ppa/+archive/ubuntu/stable) 내용을 참고하여 설치한다. <br>
  ```
   sudo add-apt-repository ppa:neovim-ppa/stable
   sudo apt update
   sudo apt install neovim
  ```
 기본 내장된 vi/vim 보다 향상된 기능을 지원하는데 lua language를 통해 Plugin 관리 뿐만 아니라 설정, 확장 등이 용이하다. <br>
 무엇보다 LSP(Language Server Protocol)을 통한 개발환경 구축이 기본 Vim과 비교할 수 없을 정도로 편리하다.
 Neovim을 직접적으로 설정하진 않을 것이고 Neovim 기반의 LunarVim 을 설치하고 환경 설정을 할 것이다. <br>
 apt-repository 추가없이 neovim을 설치할 경우 0.5.0 버젼이 설치되는데 0.7.0 버젼 기반의 Neovim Plugin들을 사용하기 위해서이다.
 
 nvim 명령을 통해 실행이 가능하며 :checkhealth 명령어를 통해 기본 환경 체크가 가능하다.

#### 3. ~/.tmux.conf 파일 설정
 iPad 에서 Termius를 통해 SSH Terminal 에 접속할 경우 화면이 작고 GUI 사용이 불가능하기에 Multitasking이 제한된다. 이 때 Tmux를 사용하면 좀 더 편리하게 Multitasking이 가능하다. <br>
 Tmux 상에서 Neovim을 사용하기 위해 몇가지 옵션들을 추가한다.
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
