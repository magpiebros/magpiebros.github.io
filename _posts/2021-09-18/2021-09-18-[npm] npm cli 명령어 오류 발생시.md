## [npm] npm cli 명령어 오류 발생시
역시 뭔가를 새로 시도하다보면, 자연스럽게 다가오는것이 에러일 것이다. 이때는 당황하지 않고, 에러를 회피하고 검색을 하면 끝.
(이걸 알아듣는다면 옛날사랆..)

## Firebase설정을 위해 npm 사용중 에러가 발생 
ERR가 발생하면서 install이 완료되지 않는다.

```
iOS_Server % npm install -g firebase-tools
npm WARN deprecated request@2.88.2: request has been deprecated, see https://github.com/request/request/issues/3142
npm WARN deprecated uuid@3.4.0: Please upgrade  to version 7 or higher.  Older versions may use Math.random() in certain circumstances, which is known to be problematic.  See https://v8.dev/blog/math-random for details.
npm WARN deprecated har-validator@5.1.5: this library is no longer supported
npm WARN checkPermissions Missing write access to /usr/local/lib/node_modules
npm ERR! code EACCES
npm ERR! syscall access
npm ERR! path /usr/local/lib/node_modules
npm ERR! errno -13
npm ERR! Error: EACCES: permission denied, access '/usr/local/lib/node_modules'
npm ERR!  [Error: EACCES: permission denied, access '/usr/local/lib/node_modules'] {
npm ERR!   errno: -13,
npm ERR!   code: 'EACCES',
npm ERR!   syscall: 'access',
npm ERR!   path: '/usr/local/lib/node_modules'
npm ERR! }
npm ERR! 
npm ERR! The operation was rejected by your operating system.
npm ERR! It is likely you do not have the permissions to access this file as the current user
npm ERR! 
npm ERR! If you believe this might be a permissions issue, please double-check the
npm ERR! permissions of the file and its containing directories, or try running
npm ERR! the command again as root/Administrator.

npm ERR! A complete log of this run can be found in:
npm ERR!     /Users/magpie/.npm/_logs/2021-09-18T14_28_16_149Z-debug.log
```


## 원인분석 
권한문제가 발생한다.
```
npm ERR! If you believe this might be a permissions issue, please double-check the
```

## 원인분석 -> 권한 해결
에러내용을 찾기 위해 문서들을 뒤적이다, 권한 문제 해결 방법을 찾았다.
다음과 같이 커맨드라인에 명령어를 전달한다. 관련 내용 링크는 아래 적어놓았다.
아래 커맨드로 해결할수 있다.

```
iOS_Server % mkdir ~/.npm-global
iOS_Server % npm config set prefix '~/.npm-global'
iOS_Server % npm config set prefix '~/.npm-global'
iOS_Server % export PATH=~/.npm-global/bin:$PATH
iOS_Server % source ~/.profile
source: no such file or directory: /Users/magpie/.profile
iOS_Server % npm install -g jshint
/Users/magpie/.npm-global/bin/jshint -> /Users/magpie/.npm-global/lib/node_modules/jshint/bin/jshint
+ jshint@2.13.1
added 31 packages from 15 contributors in 4.031s
iOS_Server % NPM_CONFIG_PREFIX=~/.npm-global
```

[npm 권한변경](https://docs.npmjs.com/resolving-eacces-permissions-errors-when-installing-packages-globally)

## 마치며
오랫만에 오직 나만의 작품을 만들어보려하니, 슬럼프가 좀 해소되는것 같다. 멋진 마무리를 기다리며 완성을 해보자.
