# temp-readme


<source>
  @type tail
  format json
  path /var/log/jenkins/jenkins.log
  pos_file /var/lib/google-fluentd/pos/jenkins-log.pos
  tag jenkins-log
  read_from_head true
</source>

<match jenkins-log>
  @type google_cloud
  buffer_chunk_limit 1m
  flush_interval 10s
  num_threads 8
  include_tag_key true
  suppress_label true
  <buffer>
    @type memory
  </buffer>
  <inject>
    resource_key @type
    resource_value jenkins
  </inject>
  <google_cloud>
    @type stackdriver_logging
    log_name jenkins-log
    project_id <YOUR_PROJECT_ID>
    keyfile <PATH_TO_KEY_FILE>
  </google_cloud>
</match>
