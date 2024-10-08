{
    "product_version": "1.0",
    "customer": "agrana",
    "container": "tenant-agrana",
    "schema_name": "agrana_staging",
    "model_run_freq": "weekly",
    "input_data": {
        "source_protocol": "dropzone",
        "source_dir": "test-dropzone",
        "data_dir": "incoming",
        "file_type": "csv",
        "storage_method": "d6",
        "move_to_processed": "Y",
        "processed_dir": "processed",
        "source_file": {
            "item": {
                "file_name": "item_master",
                "target_table": "customer_input_items",
                "optional": "N"
            },
            "sales_history": {
                "file_name": "sales_history",
                "target_table": "customer_input_sales_history",
                "optional": "N"
            },
            "customer": {
                "file_name": "customer_master",
                "target_table": "customer_input_customers",
                "optional": "N"
            },
            "location": {
                "file_name": "location_master",
                "target_table": "customer_input_locations",
                "optional": "N"
            }
        }
    },
    "transform": {
        "dbt_target": "dev",
        "dbt_project_dir": "dbt/tenant_agrana",
        "input": {
            "model_path": "models/input",
            "source": "customer_input",
            "dbt_vars": {}
        },
        "output": {
            "model_path": "use_default_model",
            "source": "customer_output",
            "dbt_vars": {}
        }
    },
    "engine_data_file": {
        "customer": {
            "table_name": "marts_customers",
            "file_name": "customers.csv",
            "data_dir": "customer_data",
            "columns": {
                "customer_group_name_c": "customer_group_name_c",
                "Billing_Country": "billing_country",
                "entity_c": "entity_c",
                "Unit_Code_3": "unit_code_3",
                "Channel": "channel",
                "Billing Country - Channel": "billing_country_channel",
                "All_Customers": "all_customers"
            }
        },
        "location": {
            "table_name": "marts_locations",
            "file_name": "locations.csv",
            "data_dir": "location_data",
            "columns": {
                "Location": "location",
                "Ldesc": "ldesc",
                "LPGRP": "lpgrp",
                "LATT1": "latt1",
                "LATT2": "latt2",
                "LATT3": "latt3",
                "All_Locations": "all_locations"
            }
        },
        "item": {
            "table_name": "marts_items",
            "file_name": "items.csv",
            "data_dir": "item_data",
            "columns": {
                "Forecasted item": "forecasted_item",
                "Product_Family_Description": "product_family_description",
                "Product_Category_Description": "product_category_description",
                "Product_Class_Description": "product_class_description",
                "Product_Line_Description": "product_line_description",
                "Product_Type_Description": "product_type_description",
                "Type_Description": "type_description",
                "Product line": "product_line",
                "Collaborative Item": "collabortive_item",
                "All_Items": "all_items"
            }
        },
        "sales_history": {
            "table_name": "marts_sales_history",
            "file_name": "sales_history.csv",
            "data_dir": "shipment_history",
            "columns": {
                "Forecasted item": "forecasted_item",
                "Location": "location",
                "customer_group_name_c": "customer_group_name_c",
                "SDATE": "sdate",
                "SQTY": "sqty"
            }
        }
    },
    "engine_run": {
        "memory": {
            "dask_worker_memory": "4Gi",
            "engine_memory": "20Gi",
            "dask_scheduler_memory": "6Gi"
        }
    },
    "forecast_export": {
        "target_storage_protocol": "dropzone",
        "export_history_to_customer": "N",
        "storage_method": "d6",
        "forecast_kind": "planner_forecast",
        "history_kind": "planner_history",
        "level": "LEVEL_GFU",
        "include_forecast_components": "Y",
        "include_historical_components": "Y",
        "aggregate": "",
        "forecast_stage_table": "forecast_export",
        "fcst_exp_gfu_map": {
            "ITEM": "Forecasted item",
            "LOC": "Location",
            "CUST": "customer_group_name_c"
        },
        "export_table": "marts_forecast_export",
        "export_header_list": [
            "VERSION",
            "ITEM",
            "LOC",
            "CUST",
            "DATE",
            "QTY",
            "BASELINE",
            "TREND",
            "SEASONALITY",
            "PROMO",
            "NON_PROMO",
            "UPPER_BOUND",
            "LOWER_BOUND",
            "ALL_OTHERS",
            "HIST_FCST_FLAG"
        ],
        "export_columns_list": [
            "VERSION",
            "ITEM",
            "LOC",
            "CUST",
            "DATE",
            "ROUND(QTY)",
            "ROUND(BASELINE)",
            "ROUND(TREND)",
            "ROUND(SEASONALITY)",
            "ROUND(PROMO)",
            "ROUND(NON_PROMO)",
            "ROUND(UPPER_BOUND)",
            "ROUND(LOWER_BOUND)",
            "ROUND(ALL_OTHERS)",
            "HIST_FCST_FLAG"
        ],
        "export_filename": "forecast_export",
        "export_filetype": "csv",
        "target_dir": "test-dropzone/outgoing"
    },
    "email_alert": {
        "email_to": [
            "csmops@garvis.ai",
            "neil.wong@logility.com",
            "valerie.divare@agrana.com",
            "maurice.shneor@agrana.com",
            "stephane.verscheure@agrana.com",
            "darshan.setty@logility.com"
        ]
    },
    "schedules": [
        {
            "type": "cron",
            "infra_size": "small",
            "expression": "0 20 * * 1-5",
            "timezone": "Asia/Kolkata",
            "name": "e2e",
            "parameters": {
                "load_data": false,
                "config_dir": "agrana",
                "run_engine": true,
                "environment": "prod",
                "housekeeping": false,
                "inv_plan_run": false,
                "dbt_vars_ovrd": {},
                "ds_run_engine": false,
                "inv_data_prep": false,
                "dp_sdfe_export": false,
                "push_inv_files": false,
                "refresh_master": false,
                "ds_housekeeping": false,
                "engine_run_mode": "incremental",
                "inv_plan_export": false,
                "notify_customer": false,
                "do_transform_data": false,
                "engine_data_dir": "onboarding/for_daily_run/engine_data",
                "engine_config_dir": "onboarding/for_daily_run/config",
                "do_export_forecast": false,
                "ds_engine_run_mode": "incremental_demand_sensing",
                "ds_export_forecast": false,
                "engine_data_export": false,
                "deployment_environment": "prod",
                "deployment_instance": "agrana.garvis.app",
                "do_export_workbench": false,
                "only_update_history": false,
                "ds_engine_config_dir": "config/DS/PROD",
                "run_data_validations": true,
                "ds_engine_data_export": false,
                "ds_deployment_instance": "",
                "fail_when_missing_file": false,
                "forecast_export_versions": "",
                "ds_deployment_environment": "",
                "incremental_version_method": "current",
                "ds_forecast_export_versions": "",
                "ds_engine_run_consume_orders": "live_orders",
                "ds_incremental_version_method": "current",
                "partial_last_bucket_shipments": false,
                "rerun_full_on_hierarchy_change": true,
                "engine_run_incremental_versions": "2023-11-27",
                "ds_engine_run_export_ds_forecasts": false,
                "ds_engine_run_incremental_versions": "",
                "ds_engine_run_ds_include_control_components": false
            }
        },
        {
            "type": "cron",
            "infra_size": "small",
            "expression": "0 20 * * 1-5",
            "timezone": "Asia/Kolkata",
            "name": "e2e_alt",
            "parameters": {
                "load_data": false,
                "config_dir": "agrana",
                "run_engine": true,
                "environment": "prod",
                "housekeeping": false,
                "inv_plan_run": false,
                "dbt_vars_ovrd": {},
                "ds_run_engine": false,
                "inv_data_prep": false,
                "dp_sdfe_export": false,
                "push_inv_files": false,
                "refresh_master": false,
                "ds_housekeeping": false,
                "engine_run_mode": "incremental",
                "inv_plan_export": false,
                "notify_customer": false,
                "do_transform_data": false,
                "engine_data_dir": "onboarding/for_daily_run_alt/engine_data",
                "engine_config_dir": "onboarding/for_daily_run_alt/config",
                "do_export_forecast": false,
                "ds_engine_run_mode": "incremental_demand_sensing",
                "ds_export_forecast": false,
                "engine_data_export": false,
                "deployment_environment": "prod",
                "deployment_instance": "agrana-alt.garvis.app",
                "do_export_workbench": false,
                "only_update_history": false,
                "ds_engine_config_dir": "config/DS/PROD",
                "run_data_validations": true,
                "ds_engine_data_export": false,
                "ds_deployment_instance": "",
                "fail_when_missing_file": false,
                "forecast_export_versions": "",
                "ds_deployment_environment": "",
                "incremental_version_method": "current",
                "ds_forecast_export_versions": "",
                "ds_engine_run_consume_orders": "live_orders",
                "ds_incremental_version_method": "current",
                "partial_last_bucket_shipments": false,
                "rerun_full_on_hierarchy_change": true,
                "engine_run_incremental_versions": "2023-11-27",
                "ds_engine_run_export_ds_forecasts": false,
                "ds_engine_run_incremental_versions": "",
                "ds_engine_run_ds_include_control_components": false
            }
        }
    ]
}
