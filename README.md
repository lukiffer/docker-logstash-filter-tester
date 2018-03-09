# Docker based logstash filter tester

* Inspired from
  * https://gquintana.github.io/2016/09/07/Testing-Logstash-configuration.html
  * https://github.com/pgporada/rspec-logstash-filter
  * https://discuss.elastic.co/t/how-to-run-rspec-tests-in-logstash-5-0-0/64895

* Uses logstash version 6.2.2
* Tests a logstash filter using an rspec test definition
* Example rspec can be found at https://github.com/pgporada/rspec-logstash-filter/blob/master/spec/filter_spec.rb,
  example config at https://github.com/pgporada/rspec-logstash-filter/blob/master/conf/filter.conf

An option is to use a docker multistage build (https://docs.docker.com/develop/develop-images/multistage-build/) to first
run tests on your filters and on success build your preconfigured logstash docker image based on tested filters.

Example:

```
FROM micwevermind/docker-logstash-filter-tester
ADD filter.conf /filter.conf
ADD filter_tests.rb /filter_tests.rb
RUN  bin/rspec --force-color /filter_tests.rb

FROM docker.elastic.co/logstash/logstash-oss:6.2.2

[...]
ADD filter.conf /usr/share/logstash/pipeline/filter.conf
[...]
```
