Edited by 0xDEADBEF
Tue, 27 Jun 2017 10:48:46 GMT
# MySql 5.7 service not start problem

MySQL 5.5에서는 mysqld로 서비스를 시작했지만 5.7에서는 `service mysql start`를 써야되었다. (모든 환경에서 그런건 아닐것이다. 나는 goorm IDE 터미널이라는 특수한 환경이었다. )
그런데 서비스를 시작하고 터미널을 종료한 뒤 다시 들어가면 서비스는 중단되어 있고 다시 `service mysql start`를 할경우 . . .만 내뱉다가 멈춰버린다.

구글링을 하면 벼러별 방법들이 나온다. 물론 내 경우에는 해결되는 방법이 없었다. 그래서 나처럼 클라우드 환경에서 MySQL 5.7을 설치하고 삽질 중인 누군가 또는 미래의 나를 위해 해결책을 남겨둔다.

```
어라 까먹었다. 미안 뭐였지;
```
