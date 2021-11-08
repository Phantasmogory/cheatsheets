```sh
#поменять параметр в конфиге оставив коммент(меняет только первое вхождение)
sed -i "s/^[#]*port[^#]*\([#]*.*\)$/port = 5433 \\1/" $PGDATA/postgresql.conf
#поменять параметр в конфиге оставив коммент(меняет все вхождения)
sed -i "s/^[#]*port[^#]*\([#]*.*\)$/port = 5433 \\1/g" $PGDATA/postgresql.conf

#интересный сниппет
aOPTION="archive_command"	
aVALUE="'\/bin\/true'"

sed -i "s/^[#]*$aOPTION[^#]*\([#]*.*\)$/$aOPTION = $aVALUE \\1/" $PGDATA/postgresql.conf | grep $aOPTION
psql -c "alter system reset $aOPTION"
psql -c "select pg_reload_conf();"
psql -c "show $aOPTION;"


```
