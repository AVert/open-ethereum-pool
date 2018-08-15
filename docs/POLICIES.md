# Enforcing Policies

Pool policy server collecting several stats on per IP basis. There are two options: `iptables+ipset` or simple application level bans. Banning is disabled by default.

## Защита фаерволом (бан)

First you need to configure your firewall to use `ipset`, read [this article](https://wiki.archlinux.org/index.php/Ipset).

Specify `ipset` name for banning in `policy` section. Timeout argument (in seconds) will be passed to this `ipset`. Stratum will use `os/exec` command like `sudo ipset add banlist x.x.x.x 1800` for banning, so you have to configure `sudo` properly and make sure that your system will never ask for password:

Example `/etc/sudoers.d/pool` where `pool` is a username under which pool runs:

    pool ALL=NOPASSWD: /sbin/ipset

If you need something simple, just set `ipset` name to blank string and simple application level banning will be used instead.

## Ограничивание

По каким то странным причинам вы можете ужесточить ограничения для защиты от атаки большого количества подключений к стратум серверу. Для этого есть изначальные настройки: `limit` and `limitJump`. Сервер безопасности (если он у вас выделеный) будет увеличивать число разрешенных соединений с одного IP адреса на для каждой отправленной валидной шары (решения). Стратум (сервер) не будет усиливать ограничения в так называемый `grace` период (период щедрости) начинающийся сразу после старта сервера.
