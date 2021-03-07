# mysql_sys.x-io_global_by_wait_by_latency-

SELECT 
    SUBSTRING_INDEX(`performance_schema`.`file_summary_by_event_name`.`EVENT_NAME`,
            '/',
            -(2)) AS `event_name`,
    `performance_schema`.`file_summary_by_event_name`.`COUNT_STAR` AS `total`,
    `performance_schema`.`file_summary_by_event_name`.`SUM_TIMER_WAIT` AS `total_latency`,
    `performance_schema`.`file_summary_by_event_name`.`AVG_TIMER_WAIT` AS `avg_latency`,
    `performance_schema`.`file_summary_by_event_name`.`MAX_TIMER_WAIT` AS `max_latency`,
    `performance_schema`.`file_summary_by_event_name`.`SUM_TIMER_READ` AS `read_latency`,
    `performance_schema`.`file_summary_by_event_name`.`SUM_TIMER_WRITE` AS `write_latency`,
    `performance_schema`.`file_summary_by_event_name`.`SUM_TIMER_MISC` AS `misc_latency`,
    `performance_schema`.`file_summary_by_event_name`.`COUNT_READ` AS `count_read`,
    `performance_schema`.`file_summary_by_event_name`.`SUM_NUMBER_OF_BYTES_READ` AS `total_read`,
    IFNULL((`performance_schema`.`file_summary_by_event_name`.`SUM_NUMBER_OF_BYTES_READ` / NULLIF(`performance_schema`.`file_summary_by_event_name`.`COUNT_READ`,
                    0)),
            0) AS `avg_read`,
    `performance_schema`.`file_summary_by_event_name`.`COUNT_WRITE` AS `count_write`,
    `performance_schema`.`file_summary_by_event_name`.`SUM_NUMBER_OF_BYTES_WRITE` AS `total_written`,
    IFNULL((`performance_schema`.`file_summary_by_event_name`.`SUM_NUMBER_OF_BYTES_WRITE` / NULLIF(`performance_schema`.`file_summary_by_event_name`.`COUNT_WRITE`,
                    0)),
            0) AS `avg_written`
FROM
    `performance_schema`.`file_summary_by_event_name`
WHERE
    ((`performance_schema`.`file_summary_by_event_name`.`EVENT_NAME` LIKE 'wait/io/file/%')
        AND (`performance_schema`.`file_summary_by_event_name`.`COUNT_STAR` > 0))
ORDER BY `performance_schema`.`file_summary_by_event_name`.`SUM_TIMER_WAIT` DESC
