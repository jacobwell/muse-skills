# muse.db Structure — tables and columns

195 tables across 17 schemas. `[PK]` = primary key, `[FK → table]` = foreign key, `?` = nullable.

## activity — 8 tables

### activity.activity_monitor_agent_threads
- `agent_id`: `text` [PK]
- `activity_thread_id`: `text`
- `created_at`: `text`
- `updated_at`: `text`

### activity.activity_monitor_carrier_user_messages
- `link_seq`: `bigint` [PK]
- `carrier_message_id`: `text`
- `user_message_id`: `text`
- `root_agent_id`: `text` [null]
- `created_at`: `timestamp with time zone`

### activity.activity_monitor_message_threads
- `message_id`: `text` [PK]
- `activity_thread_id`: `text`
- `created_at`: `text`
- `updated_at`: `text`

### activity.activity_monitor_runtime_work_threads
- `work_id`: `text` [PK]
- `activity_thread_id`: `text` [FK→activity.activity_monitor_threads]
- `created_at`: `text`
- `updated_at`: `text`

### activity.activity_monitor_thread_actions
- `action_seq`: `bigint`
- `action_id`: `text` [PK]
- `activity_thread_id`: `text`
- `action_index`: `bigint`
- `agent_id`: `text` [null]
- `agent_depth`: `integer` [null]
- `icon`: `text`
- `title`: `text`
- `subtitle`: `text` [null]
- `report`: `text`
- `status`: `text`
- `created_at`: `text`
- `section_key`: `text` [null]
- `section_title`: `text` [null]
- `section_order`: `integer` [null]
- `section_depth`: `integer` [null]
- `parent_section_key`: `text` [null]
- `ordinal_label`: `text` [null]

### activity.activity_monitor_thread_sections
- `activity_thread_id`: `text` [PK, FK→activity.activity_monitor_threads]
- `section_key`: `text`
- `title`: `text`
- `status`: `text`
- `section_order`: `integer` [null]
- `section_depth`: `integer` [null]
- `parent_section_key`: `text` [null]
- `ordinal_label`: `text` [null]
- `created_at`: `text`
- `updated_at`: `text`

### activity.activity_monitor_threads
- `activity_thread_id`: `text` [PK]
- `name`: `text`
- `subtitle`: `text`
- `status_title`: `text` [null]
- `icon`: `text`
- `emoji`: `text` [null]
- `activity_thread_kind`: `text` [null]
- `space_slug`: `text` [null]
- `expected_finish_description`: `text` [null]
- `status`: `text`
- `finish_status`: `text` [null]
- `finish_status_reason`: `text` [null]
- `created_at`: `text`
- `finished_at`: `text` [null]
- `finish_message`: `text` [null]
- `artifact_slug`: `text` [null]

### activity.feed_entries
- `activity_key`: `text` [PK]
- `activity_type`: `text`
- `is_goal`: `boolean`
- `message_id`: `text` [null]
- `title`: `text` [null]
- `status_title`: `text` [null]
- `subtitle`: `text` [null]
- `details_json`: `text` [null]
- `status`: `text`
- `task_label`: `text` [null]
- `created_at_ms`: `bigint`
- `updated_at_ms`: `bigint`
- `finished_at_ms`: `bigint` [null]
- `created_at`: `timestamp with time zone` [null]
- `updated_at`: `timestamp with time zone` [null]
- `finished_at`: `timestamp with time zone` [null]

## agent — 26 tables

### agent.agent_ancestors
- `descendant_id`: `text` [PK]
- `ancestor_id`: `text`
- `distance`: `integer`

### agent.agent_compactions
- `id`: `bigint` [PK]
- `agent_id`: `text`
- `summary`: `text`
- `first_kept_seq`: `bigint` [null]
- `checkpoint_seq`: `bigint` [null]
- `tokens_before`: `bigint` [null]
- `tokens_after`: `bigint` [null]
- `trigger`: `text`
- `will_retry`: `boolean`
- `created_at`: `bigint`
- `has_replacement_history`: `boolean`

### agent.agent_message_token_usage
- `agent_id`: `text` [PK]
- `message_id`: `text`
- `input_tokens`: `bigint`
- `output_tokens`: `bigint`
- `created_at`: `bigint`

### agent.agents
- `agent_id`: `text` [PK]
- `id`: `text` [null]
- `session_id`: `text` [null]
- `kind`: `text`
- `parent_id`: `text` [null]
- `parent_agent_id`: `text` [null]
- `root_session_id`: `text` [null]
- `request_id`: `text` [FK→runtime.requests, null]
- `model`: `text`
- `status`: `text`
- `depth`: `integer`
- `created_at`: `bigint`
- `updated_at`: `bigint`
- `last_assistant_message`: `text` [null]
- `seen_at_ms`: `bigint` [null]
- `prompt_floor_seq`: `bigint` [null]
- `ephemeral`: `boolean`
- `last_assistant_resources`: `text` [null]
- `agent_type`: `text` [null]
- `presentation_root_session_id`: `text` [null]
- `spawn_metadata_json`: `text` [null]
- `originating_location_context_json`: `text` [null]

### agent.chat_preferences
- `id`: `integer` [PK]
- `active_root_id`: `text`
- `verbose`: `bigint`
- `usage_mode`: `text`
- `active_root_cleared_at_ms`: `bigint` [null]

### agent.compactions
- `compaction_id`: `bigint` [PK]
- `agent_id`: `text`
- `checkpoint_context_item_id`: `bigint` [FK→agent.context_items, null]
- `replacement_history_first_seq`: `integer` [null]
- `replacement_history_last_seq`: `integer` [null]
- `created_at`: `timestamp with time zone`
- `summary_text`: `text`

### agent.context_item_derived_write_backlog
- `context_item_id`: `bigint` [PK, FK→agent.context_items]
- `attempts`: `integer`
- `last_error`: `text` [null]
- `created_at`: `timestamp with time zone`
- `updated_at`: `timestamp with time zone`
- `deadlettered_at`: `timestamp with time zone` [null]

### agent.context_item_fields
- `context_item_field_id`: `bigint` [PK]
- `context_item_id`: `bigint` [FK→agent.context_items]
- `field_path`: `text`
- `scalar_type`: `text`
- `scalar_value`: `text` [null]
- `field_text_content`: `text` [null]

### agent.context_item_resume_projection
- `agent_id`: `text` [PK]
- `seq`: `bigint`
- `context_item_id`: `bigint` [FK→agent.context_items, null]
- `created_at`: `bigint` [null]
- `message_source`: `text` [null]
- `client_context_json`: `text` [null]
- `reply_prefix`: `text` [null]
- `provenance_json`: `text` [null]
- `client_context_projected`: `boolean`
- `reply_prefix_projected`: `boolean`
- `provenance_projected`: `boolean`
- `updated_at`: `timestamp with time zone`

### agent.context_items
- `context_item_id`: `bigint` [PK]
- `agent_id`: `text`
- `seq`: `bigint`
- `item_kind`: `agent.context_item_kind`
- `message_id`: `text` [FK→runtime.messages, null]
- `tool_call_id`: `bigint` [FK→runtime.tool_calls, null]
- `tool_output_id`: `bigint` [FK→runtime.tool_outputs, null]
- `role`: `runtime.message_role` [null]
- `call_id`: `text` [null]
- `tool_name`: `text` [null]
- `success`: `boolean` [null]
- `message_source`: `text` [null]
- `created_at`: `timestamp with time zone`
- `data_fbids`: `bigint[]` [null]
- `data_message_ids`: `text[]` [null]
- `data_cleanup_checked_at`: `timestamp with time zone` [null]
- `data_summarized_at`: `timestamp with time zone` [null]
- `data_thread_ids`: `text[]` [null]
- `data_expires_at`: `timestamp with time zone` [null]
- `text_content`: `text` [null]
- `item_json`: `text` [null]

### agent.context_text_segments
- `context_text_segment_id`: `bigint` [PK]
- `context_item_id`: `bigint` [FK→agent.context_items]
- `ordinal`: `integer`
- `text_content`: `text`

### agent.message_mailbox
- `submission_id`: `text` [PK]
- `accepted_ordinal`: `bigint`
- `agent_id`: `text` [FK→agent.agents]
- `conversation_epoch`: `bigint`
- `submission_kind`: `text`
- `routing_scope`: `text`
- `state`: `text`
- `stream_owner_message_id`: `text` [null]
- `submission_json`: `text`
- `transcript_event_seq`: `bigint` [FK→runtime.events, null]
- `accepted_at`: `timestamp with time zone`
- `attached_at`: `timestamp with time zone` [null]
- `terminalized_at`: `timestamp with time zone` [null]
- `checkpoint_custody`: `text` [null]

### agent.recovery_owner_terminal_events
- `recovery_class`: `text` [PK]
- `owner_id`: `text`
- `generation`: `bigint`
- `event_type`: `text`
- `claimed_at`: `timestamp with time zone`

### agent.recovery_owners
- `recovery_class`: `text` [PK]
- `owner_id`: `text`
- `generation`: `bigint`
- `agent_id`: `text`
- `message_id`: `text`
- `active_at`: `timestamp with time zone`
- `terminal_at`: `timestamp with time zone` [null]
- `terminal_status`: `text` [null]

### agent.runtime_restart_checkpoints
- `agent_id`: `text` [PK]
- `mode`: `text`
- `payload_json`: `text` [null]
- `created_at`: `bigint`
- `updated_at`: `bigint`
- `recovery_class`: `text` [null]
- `recovery_owner_id`: `text` [null]
- `execution_config_json`: `text` [null]

### agent.runtime_state
- `agent_id`: `text` [PK]
- `current_context_seq`: `integer`
- `last_event_seq`: `bigint` [FK→runtime.events, null]
- `updated_at`: `timestamp with time zone`

### agent.session_memory_capture_deadlines
- `root_session_id`: `text` [PK, FK→agent.agents]
- `session_id`: `text` [null]
- `reason`: `text`
- `due_at_ms`: `bigint`
- `created_at_ms`: `bigint`
- `updated_at_ms`: `bigint`

### agent.session_metadata
- `session_id`: `text` [PK]
- `origin`: `text`
- `lifecycle`: `text`
- `status`: `text`
- `thread_title`: `text` [null]
- `source_session_id`: `text` [null]
- `source_prompt_seq_upper_bound`: `bigint` [null]
- `source_message_id_boundary`: `text` [null]
- `created_at`: `bigint`
- `updated_at`: `bigint`
- `pinned`: `boolean`
- `channel`: `text` [null]
- `channel_conversation_id`: `text` [null]
- `channel_delivery_target`: `text` [null]
- `pinned_order`: `bigint` [null]

### agent.sessions
- `session_id`: `text` [PK]
- `root_request_id`: `text` [FK→runtime.requests, null]
- `created_at`: `timestamp with time zone`
- `updated_at`: `timestamp with time zone`
- `archived_at`: `timestamp with time zone` [null]

### agent.subagent_monitor_decisions
- `decision_id`: `bigint` [PK]
- `coordinator_agent_id`: `text`
- `child_agent_id`: `text`
- `parent_agent_id`: `text`
- `parent_message_id`: `text`
- `monitor_event_id`: `bigint`
- `monitor_event_kind`: `text`
- `inference_request_id`: `text`
- `assistant_text`: `text` [null]
- `tool_call_id`: `text` [null]
- `tool_name`: `text` [null]
- `tool_arguments`: `text` [null]
- `tool_result_json`: `text`
- `decision_kind`: `text`
- `created_at`: `bigint`

### agent.subagent_progress
- `progress_id`: `bigint` [PK]
- `child_agent_id`: `text`
- `event_seq`: `bigint` [FK→runtime.events, null]
- `status`: `text`
- `created_at`: `timestamp with time zone`
- `progress_text`: `text` [null]

### agent.subagent_progress_message_events
- `event_id`: `bigint` [PK]
- `child_agent_id`: `text`
- `parent_agent_id`: `text`
- `parent_message_id`: `text`
- `message_text`: `text`
- `created_at`: `bigint`
- `delivered_at`: `bigint` [null]

### agent.subagent_progress_tool_events
- `event_id`: `bigint` [PK]
- `child_agent_id`: `text`
- `parent_agent_id`: `text`
- `parent_message_id`: `text`
- `tool_name`: `text`
- `tool_description`: `text`
- `tool_status`: `text`
- `tool_result_preview`: `text` [null]
- `created_at`: `bigint`
- `delivered_at`: `bigint` [null]
- `run_id`: `text` [null]

### agent.subagent_spawns
- `spawn_id`: `bigint` [PK]
- `parent_agent_id`: `text`
- `child_agent_id`: `text`
- `parent_message_id`: `text` [null]
- `parent_request_id`: `text` [null]
- `root_request_id`: `text` [null]
- `root_message_execution_id`: `text` [null]
- `request_id`: `text` [FK→runtime.requests, null]
- `created_at`: `bigint`
- `status`: `text` [null]
- `deferred_terminal_status`: `text` [null]
- `final_response`: `text` [null]
- `completed_at`: `bigint` [null]
- `prompt`: `text` [null]
- `requester_source`: `text` [null]
- `requester_channel_context_json`: `text` [null]
- `seen_at`: `bigint` [null]
- `child_depth`: `integer`
- `agent_type`: `text` [null]
- `metadata_json`: `text` [null]
- `spawn_call_id`: `text` [null]

### agent.token_usage
- `token_usage_id`: `bigint` [PK]
- `agent_id`: `text`
- `request_id`: `text` [FK→runtime.requests, null]
- `input_tokens`: `integer`
- `output_tokens`: `integer`
- `cached_input_tokens`: `integer`
- `reasoning_tokens`: `integer`
- `created_at`: `timestamp with time zone`

### agent.volatile_context_pins
- `agent_id`: `text` [PK]
- `variant_hash`: `text`
- `variant_json`: `text`
- `prompt_floor_seq`: `bigint`
- `checkpoint_seq`: `bigint`
- `body`: `text` [null]
- `created_at`: `timestamp with time zone`

## device — 13 tables

### device.calendar_events
- `node_id`: `text` [PK, FK→device.nodes]
- `external_event_id`: `text`
- `title`: `text`
- `start_at`: `timestamp with time zone`
- `end_at`: `timestamp with time zone`
- `is_all_day`: `boolean`
- `external_calendar_id`: `text`
- `calendar_name`: `text`
- `availability`: `text`
- `location`: `text` [null]
- `notes`: `text` [null]
- `time_zone`: `text` [null]
- `start_local_date`: `date` [null]
- `end_local_date`: `date` [null]
- `is_recurring`: `boolean`
- `url`: `text` [null]
- `event_status`: `text`
- `calendar_color`: `text` [null]

### device.call_log
- `call_log_id`: `bigint` [PK]
- `producer_id`: `text`
- `external_id`: `text`
- `phone_number`: `text` [null]
- `contact_name`: `text` [null]
- `call_type`: `text` [null]
- `occurred_at_unix_ms`: `bigint` [null]
- `duration_seconds`: `bigint` [null]
- `platform`: `text` [null]
- `created_at`: `timestamp with time zone`

### device.client_contexts
- `connection_id`: `text` [PK]
- `device_id`: `text`
- `session_id`: `text` [null]
- `root_session_id`: `text` [null]
- `platform`: `text` [null]
- `view`: `text` [null]
- `mode`: `text` [null]
- `is_visible`: `boolean` [null]
- `presence_status`: `text`
- `metadata_json`: `jsonb` [null]
- `connected_at_ms`: `bigint`
- `last_active_at_ms`: `bigint`
- `updated_at_ms`: `bigint`
- `created_at`: `timestamp with time zone`

### device.contact_addresses
- `contact_address_id`: `bigint` [PK]
- `contact_id`: `bigint` [FK→device.contacts]
- `label`: `text` [null]
- `street`: `text` [null]
- `city`: `text` [null]
- `state`: `text` [null]
- `postal_code`: `text` [null]
- `country`: `text` [null]

### device.contact_emails
- `contact_email_id`: `bigint` [PK]
- `contact_id`: `bigint` [FK→device.contacts]
- `label`: `text` [null]
- `email`: `text`

### device.contact_phones
- `contact_phone_id`: `bigint` [PK]
- `contact_id`: `bigint` [FK→device.contacts]
- `label`: `text` [null]
- `phone_e164`: `text` [null]
- `phone_raw`: `text`

### device.contacts
- `contact_id`: `bigint` [PK]
- `node_id`: `text` [FK→device.nodes]
- `platform`: `text`
- `external_contact_id`: `text`
- `display_name`: `text` [null]
- `given_name`: `text` [null]
- `family_name`: `text` [null]
- `organization`: `text` [null]
- `job_title`: `text` [null]
- `birthday_text`: `text` [null]
- `note`: `text` [null]
- `synced_at_text`: `text`
- `deleted_at`: `timestamp with time zone` [null]
- `updated_at`: `timestamp with time zone`
- `thumbnail`: `text` [null]

### device.data_sync_state
- `node_id`: `text` [PK, FK→device.nodes]
- `data_source`: `text`
- `next_full_sync_at`: `timestamp with time zone` [null]
- `current_full_sync_id`: `text` [null]
- `current_full_sync_expires_at`: `timestamp with time zone` [null]
- `current_full_sync_range_start`: `timestamp with time zone` [null]
- `current_full_sync_range_end`: `timestamp with time zone` [null]
- `changed_during_full_sync`: `boolean`
- `last_full_sync_at`: `timestamp with time zone` [null]
- `last_full_sync_range_start`: `timestamp with time zone` [null]
- `last_full_sync_range_end`: `timestamp with time zone` [null]
- `search_trigger_pending`: `boolean`
- `current_full_sync_requester_root_session_id`: `text` [null]
- `current_full_sync_requester_presentation_locale`: `text` [null]

### device.media_upload_batches
- `lane`: `text` [PK]
- `batch_id`: `text`
- `high_water_global_seq`: `bigint`
- `pending_count`: `bigint`
- `oldest_received_at_unix_ms`: `bigint`
- `claimed_at`: `timestamp with time zone`

### device.media_upload_events
- `media_upload_event_id`: `bigint` [PK]
- `global_seq`: `bigint`
- `event_id`: `text`
- `media_id`: `text`
- `node_id`: `text` [FK→device.nodes, null]
- `lane`: `text`
- `received_at_text`: `text`
- `received_at_unix_ms`: `bigint`
- `received_at`: `timestamp with time zone`
- `status`: `text`
- `processed_at_text`: `text` [null]
- `processed_at_unix_ms`: `bigint` [null]
- `processed_at`: `timestamp with time zone` [null]
- `handoff_message_id`: `text` [null]
- `summary_preview`: `text` [null]
- `failure_code`: `text` [null]
- `failure_message`: `text` [null]

### device.nodes
- `node_id`: `text` [PK]
- `node_kind`: `text`
- `display_name`: `text` [null]
- `platform`: `text`
- `enabled_permissions_json`: `text`
- `commands_json`: `text`
- `version`: `text` [null]
- `device_family`: `text` [null]
- `model_id`: `text` [null]
- `is_wakeup_supported`: `boolean` [null]
- `delivery_app`: `text` [null]
- `paired_at_text`: `text` [null]
- `created_at`: `timestamp with time zone`
- `last_seen_at_text`: `text` [null]
- `last_seen_at`: `timestamp with time zone` [null]
- `status`: `text`
- `revoked`: `boolean`
- `contacts_last_synced_at`: `text` [null]
- `contacts_last_received_at`: `text` [null]
- `contacts_last_sync_status`: `text`
- `contacts_last_sync_error`: `text` [null]
- `contacts_contact_count`: `bigint`
- `health_last_received_at`: `text` [null]
- `health_last_sync_status`: `text`
- `health_last_sync_error`: `text` [null]
- `data_sources_json`: `text`
- `invoke_protocol`: `text`
- `pairing_status`: `text`
- `contacts_content_sha256`: `text` [null]
- `metadata_json`: `text`
- `location_sharing_mode`: `text` [null]

### device.upload_chunks
- `upload_session_id`: `text` [PK, FK→device.upload_sessions]
- `chunk_index`: `integer`
- `payload_digest`: `text`
- `created_at_text`: `text`
- `created_at`: `timestamp with time zone`
- `payload`: `text`

### device.upload_sessions
- `upload_session_id`: `text` [PK]
- `node_id`: `text`
- `route_kind`: `text`
- `request_id`: `text` [null]
- `datatype`: `text`
- `sync_mode`: `text` [null]
- `chunk_count`: `integer`
- `status`: `text`
- `created_at_text`: `text`
- `updated_at_text`: `text`
- `expires_at_text`: `text`
- `expires_at`: `timestamp with time zone`
- `created_at`: `timestamp with time zone`
- `response`: `text` [null]

## feed — 16 tables

### feed.fleet_engagement_contributions
- `contribution_id`: `text` [PK]
- `origin_id`: `text`
- `engagement_version`: `bigint`
- `created_at_ms`: `bigint`
- `updated_at_ms`: `bigint`

### feed.fleet_engagement_outbox
- `contribution_id`: `text` [PK]
- `origin_id`: `text`
- `deleted_count`: `bigint`
- `discuss_count`: `bigint`
- `share_count`: `bigint`
- `seed_use_count`: `bigint`
- `mutation_version`: `bigint`
- `attempt_count`: `integer`
- `next_attempt_at_ms`: `bigint`
- `created_at_ms`: `bigint`
- `updated_at_ms`: `bigint`

### feed.fleet_fetch_receipts
- `payload_hash`: `text` [PK]
- `first_fetched_at_ms`: `bigint`
- `origin_id`: `text` [null]

### feed.fleet_publish_state
- `singleton`: `boolean` [PK]
- `published_watermark_ms`: `bigint`

### feed.fleet_reaction_outbox
- `unit_id`: `text` [PK]
- `origin_id`: `text`
- `liked`: `boolean`
- `mutation_version`: `bigint`
- `attempt_count`: `integer`
- `next_attempt_at_ms`: `bigint`
- `created_at_ms`: `bigint`
- `updated_at_ms`: `bigint`

### feed.interactions
- `interaction_id`: `text` [PK]
- `unit_id`: `text`
- `kind`: `text`
- `value`: `text` [null]
- `created_at_ms`: `bigint`

### feed.null_state_seed
- `singleton`: `boolean` [PK]
- `seeded_at_ms`: `bigint`

### feed.preferences_projection
- `singleton_id`: `smallint` [PK]
- `preferences_md`: `text`
- `folded_through_ms`: `bigint`
- `updated_by_run`: `text`
- `updated_at_ms`: `bigint`

### feed.prompt_scope_verdict
- `singleton`: `boolean` [PK]
- `prompt_sha256`: `text`
- `needs_interpretation`: `boolean`
- `reason`: `text`
- `classifier_version`: `integer`
- `classified_at_ms`: `bigint`

### feed.prompt_seed
- `singleton`: `boolean` [PK]
- `seeded_at_ms`: `bigint`

### feed.promptless_unit_orders
- `prompt_id`: `text` [PK]
- `unit_id`: `text` [FK→feed.units]
- `local_date`: `date`
- `manual_order`: `double precision`

### feed.prompts
- `prompt_id`: `text` [PK]
- `template_id`: `text` [null]
- `slot_values`: `jsonb`
- `free_text`: `text`
- `enabled`: `boolean`
- `created_at_ms`: `bigint`
- `updated_at_ms`: `bigint`
- `slot_ingredients`: `jsonb`
- `generation_queued_at_ms`: `bigint` [null]
- `full_prompt`: `text` [null]
- `generation_queued_requester`: `jsonb` [null]

### feed.run_steps
- `run_id`: `text` [PK]
- `step_id`: `text`
- `attempt`: `integer`
- `input_hash`: `text`
- `status`: `text`
- `agent_id`: `text` [null]
- `message_id`: `text` [null]
- `output_json`: `jsonb`
- `error`: `text` [null]
- `started_at_ms`: `bigint`
- `finished_at_ms`: `bigint` [null]
- `updated_at_ms`: `bigint`
- `submitted_at_ms`: `bigint` [null]

### feed.runs
- `run_id`: `text` [PK]
- `trigger`: `text`
- `edition_kind`: `text`
- `local_date`: `date`
- `tz`: `text`
- `prompt_id`: `text` [null]
- `prompt_snapshot`: `text` [null]
- `requester`: `jsonb` [null]
- `slot_fulfillment`: `jsonb`
- `interactions_watermark_ms`: `bigint` [null]
- `status`: `text`
- `failure_reason`: `text` [null]
- `started_at_ms`: `bigint` [null]
- `finished_at_ms`: `bigint` [null]
- `created_at_ms`: `bigint`
- `updated_at_ms`: `bigint`
- `hour_slot`: `smallint` [null]
- `search_query_keys`: `text[]` [null]

### feed.surface_state
- `singleton`: `boolean` [PK]
- `first_fetched_at_ms`: `bigint`

### feed.units
- `unit_id`: `text` [PK]
- `run_id`: `text`
- `prompt_id`: `text` [null]
- `edition_kind`: `text`
- `edition_local_date`: `date`
- `edition_generated_at_ms`: `bigint`
- `position`: `integer`
- `kicker`: `text`
- `body_md`: `text`
- `attachment_kind`: `text`
- `header_image_path`: `text` [null]
- `image_urls`: `text[]` [null]
- `widget_html`: `text` [null]
- `social_embed_url`: `text` [null]
- `category`: `text`
- `connector_attribution`: `text` [null]
- `stats`: `jsonb` [null]
- `reaction`: `text` [null]
- `reaction_updated_at_ms`: `bigint` [null]
- `share_count`: `bigint`
- `discuss_count`: `bigint`
- `origin`: `text`
- `share_instructions`: `jsonb` [null]
- `share_artifact_generated_at_ms`: `bigint` [null]
- `share_artifact_stale`: `boolean`
- `created_at_ms`: `bigint`
- `updated_at_ms`: `bigint`
- `embedding`: `vector(384)` [null]
- `manual_order`: `double precision` [null]
- `seen_at_ms`: `bigint` [null]
- `title`: `text` [null]
- `search_vector`: `tsvector` [null]
- `last_seen_at_ms`: `bigint` [null]
- `timespent_ms`: `bigint` [null]
- `social_thumbnail_url`: `text` [null]
- `social_thumbnail_aspect_ratio`: `double precision` [null]
- `social_attribution_text`: `text` [null]
- `social_post_username`: `text` [null]
- `why_did_i_see_this`: `text` [null]
- `source_post_url`: `text` [null]
- `video_media_path`: `text` [null]
- `carousel_clip_paths`: `text[]` [null]
- `source_fleet_origin_id`: `text` [null]
- `fleet_reaction_version`: `bigint`
- `emoji`: `text` [null]
- `source_idea_id`: `text` [null]
- `icon_key`: `text` [null]
- `source_url_keys`: `text[]` [null]
- `activity_tier`: `text` [null]

## goals — 12 tables

### goals.actions
- `action_id`: `bigint` [PK]
- `goal_id`: `text` [FK→goals.goals]
- `status`: `text`
- `created_at`: `timestamp with time zone`
- `updated_at`: `timestamp with time zone`
- `action_text`: `text`

### goals.associations
- `association_id`: `text` [PK]
- `goal_id`: `text` [FK→goals.goals]
- `association_type`: `text`
- `target_id`: `text`
- `details_json`: `text`
- `created_at`: `text`
- `updated_at`: `text`
- `created_at_ts`: `timestamp with time zone` [null]
- `updated_at_ts`: `timestamp with time zone` [null]

### goals.briefings
- `briefing_id`: `text` [PK]
- `goal_id`: `text` [FK→goals.goals]
- `path`: `text`
- `created_at`: `text`
- `updated_at`: `text`
- `last_opened_at`: `text` [null]
- `created_at_ts`: `timestamp with time zone` [null]
- `updated_at_ts`: `timestamp with time zone` [null]
- `last_opened_at_ts`: `timestamp with time zone` [null]
- `hero_image_path`: `text` [null]

### goals.engagement_events
- `event_id`: `text` [PK]
- `goal_id`: `text` [FK→goals.goals]
- `event_type`: `text`
- `surface`: `text` [null]
- `value`: `double precision` [null]
- `request_id`: `text`
- `metadata`: `jsonb`
- `created_at`: `timestamp with time zone`

### goals.goals
- `goal_id`: `text` [PK]
- `activity_key`: `text` [null]
- `slug`: `text` [null]
- `title`: `text`
- `summary`: `text` [null]
- `description`: `text` [null]
- `momentum`: `text` [null]
- `momentum_status`: `text` [null]
- `emoji`: `text` [null]
- `image_relpath`: `text` [null]
- `icon_generation_status`: `text` [null]
- `status`: `text`
- `created_at`: `text`
- `updated_at`: `text`
- `parent_goal_id`: `text` [FK→goals.goals, null]
- `sort_order`: `double precision`
- `completed_at`: `text` [null]
- `created_at_ts`: `timestamp with time zone` [null]
- `updated_at_ts`: `timestamp with time zone` [null]
- `last_activity_at`: `text` [null]
- `last_activity_at_ts`: `timestamp with time zone` [null]
- `pushable`: `boolean`
- `category`: `text` [null]
- `goal_kind`: `text` [null]
- `value_alignment`: `text` [null]
- `user_words`: `text` [null]
- `assistant_distillation`: `text` [null]
- `woop`: `jsonb` [null]
- `implementation_intentions`: `jsonb` [null]
- `monitoring_signal`: `text` [null]
- `review_cadence`: `text` [null]
- `next_review_question`: `text` [null]
- `momentum_dimensions`: `jsonb` [null]
- `adjustment_recommendation`: `text` [null]
- `provenance`: `jsonb` [null]
- `completed_at_ts`: `timestamp with time zone` [null]
- `source`: `text`
- `attention_kind`: `text`
- `attention_updated_at`: `timestamp with time zone`
- `next_due_at`: `timestamp with time zone` [null]
- `escalation`: `jsonb` [null]
- `escalation_due_at`: `timestamp with time zone` [null]
- `fixed_position`: `bigint` [null]

### goals.learning_state
- `goal_id`: `text` [PK, FK→goals.goals]
- `target_skill`: `text` [null]
- `prerequisites`: `jsonb`
- `mastery_estimate`: `double precision` [null]
- `mastery_evidence`: `jsonb`
- `misconceptions`: `jsonb`
- `last_retrieval_at`: `timestamp with time zone` [null]
- `next_review_at`: `timestamp with time zone` [null]
- `confidence`: `double precision` [null]
- `transfer_status`: `text` [null]
- `study_status`: `text`
- `last_studied_at`: `timestamp with time zone` [null]
- `updated_at_ts`: `timestamp with time zone`

### goals.momentum_history
- `history_id`: `bigint` [PK]
- `goal_id`: `text` [FK→goals.goals]
- `momentum_status`: `text` [null]
- `momentum_dimensions`: `jsonb` [null]
- `adjustment_recommendation`: `text` [null]
- `source`: `text`
- `run_id`: `text` [null]
- `observed_at`: `timestamp with time zone`
- `created_at`: `timestamp with time zone`

### goals.sessions
- `root_goal_id`: `text` [PK, FK→goals.goals]
- `root_agent_id`: `text`
- `session_id`: `text`

### goals.suggestions
- `suggestion_id`: `text` [PK]
- `goal_id`: `text` [FK→goals.goals]
- `idea_id`: `text`
- `status`: `text`
- `created_at`: `text`
- `updated_at`: `text`
- `created_at_ts`: `timestamp with time zone` [null]
- `updated_at_ts`: `timestamp with time zone` [null]

### goals.thread_actions
- `thread_action_id`: `bigint` [PK]
- `thread_id`: `bigint` [FK→goals.threads]
- `action_id`: `bigint` [FK→goals.actions, null]
- `created_at`: `timestamp with time zone`

### goals.threads
- `thread_id`: `bigint` [PK]
- `goal_id`: `text` [FK→goals.goals]
- `request_id`: `text` [FK→runtime.requests, null]
- `created_at`: `timestamp with time zone`

### goals.updates
- `update_id`: `text` [PK]
- `goal_id`: `text` [FK→goals.goals]
- `title`: `text` [null]
- `summary`: `text` [null]
- `description`: `text` [null]
- `effective_at`: `text` [null]
- `created_at`: `text`
- `updated_at`: `text`
- `effective_at_ts`: `timestamp with time zone` [null]
- `updated_at_ts`: `timestamp with time zone` [null]
- `author_source`: `text`

## health — 8 tables

### health.aggregates
- `aggregate_id`: `bigint` [PK]
- `provider`: `text`
- `node_id`: `text` [null]
- `timezone`: `text` [null]
- `aggregate_type`: `text`
- `period_start`: `timestamp with time zone`
- `period_end`: `timestamp with time zone`
- `numeric_value`: `double precision` [null]
- `unit`: `text` [null]

### health.events
- `health_event_id`: `bigint` [PK]
- `provider`: `text`
- `node_id`: `text` [null]
- `external_event_id`: `text` [null]
- `bundle_id`: `text` [null]
- `timezone`: `text` [null]
- `event_type`: `text`
- `event_at`: `timestamp with time zone`
- `event_end_at`: `timestamp with time zone` [null]
- `event_text`: `text` [null]

### health.record_values
- `record_value_id`: `bigint` [PK]
- `record_table`: `text`
- `record_id`: `bigint`
- `value_name`: `text`
- `unit`: `text` [null]
- `numeric_value`: `double precision` [null]
- `text_value`: `text` [null]

### health.sample_values
- `sample_value_id`: `bigint` [PK]
- `sample_id`: `bigint` [FK→health.samples]
- `value_name`: `text`
- `unit`: `text` [null]
- `numeric_value`: `double precision` [null]
- `text_value`: `text` [null]

### health.samples
- `sample_id`: `bigint` [PK]
- `provider`: `text`
- `node_id`: `text` [null]
- `external_sample_id`: `text`
- `bundle_id`: `text` [null]
- `sample_type`: `text`
- `start_at`: `timestamp with time zone`
- `end_at`: `timestamp with time zone` [null]
- `unit`: `text` [null]
- `numeric_value`: `double precision` [null]
- `text_value`: `text` [null]
- `source_name`: `text` [null]
- `created_at`: `timestamp with time zone`
- `timezone`: `text` [null]

### health.sleep_sessions
- `sleep_session_id`: `bigint` [PK]
- `provider`: `text`
- `node_id`: `text` [null]
- `external_sleep_id`: `text`
- `bundle_id`: `text` [null]
- `timezone`: `text` [null]
- `start_at`: `timestamp with time zone`
- `end_at`: `timestamp with time zone`
- `quality_score`: `double precision` [null]
- `awake_seconds`: `double precision` [null]
- `core_seconds`: `double precision` [null]
- `deep_seconds`: `double precision` [null]
- `rem_seconds`: `double precision` [null]
- `asleep_unspecified_seconds`: `double precision` [null]
- `asleep_seconds`: `integer` [null]
- `in_bed_seconds`: `integer` [null]
- `created_at`: `timestamp with time zone`

### health.synced_ranges
- `synced_range_id`: `bigint` [PK]
- `provider`: `text`
- `node_id`: `text` [null]
- `category`: `text`
- `span`: `tstzrange`
- `updated_at`: `timestamp with time zone`

### health.workouts
- `workout_id`: `bigint` [PK]
- `provider`: `text`
- `node_id`: `text` [null]
- `external_workout_id`: `text`
- `bundle_id`: `text` [null]
- `timezone`: `text` [null]
- `workout_type`: `text`
- `start_at`: `timestamp with time zone`
- `end_at`: `timestamp with time zone` [null]
- `active_seconds`: `double precision` [null]
- `energy_kcal`: `double precision` [null]
- `distance_meters`: `double precision` [null]
- `hr_max_bpm`: `double precision` [null]
- `hr_average_bpm`: `double precision` [null]
- `created_at`: `timestamp with time zone`

## ideas — 22 tables

### ideas.bandit_arm_state
- `policy_id`: `text` [PK, FK→ideas.explore_policy]
- `domain`: `text`
- `lane`: `text`
- `decision_points`: `bigint`
- `terminal_rewards`: `bigint`
- `terminal_observations`: `bigint`
- `fast_reward_sum`: `double precision`
- `fast_observations`: `bigint`
- `updated_at`: `timestamp with time zone`

### ideas.bandit_fold_state
- `policy_id`: `text` [PK, FK→ideas.explore_policy]
- `folded_until`: `timestamp with time zone` [null]
- `folded_event_id`: `text` [null]
- `updated_at`: `timestamp with time zone`

### ideas.bandit_folded_events
- `policy_id`: `text` [PK, FK→ideas.explore_policy]
- `event_id`: `text` [FK→ideas.idea_events]
- `folded_at`: `timestamp with time zone`

### ideas.discovery_pool_history
- `snapshot_id`: `text` [PK]
- `source`: `text`
- `payload_json`: `jsonb`
- `created_at`: `timestamp with time zone`

### ideas.discovery_pool_meta
- `key`: `text` [PK]
- `source`: `text`
- `payload_json`: `jsonb`
- `updated_at`: `timestamp with time zone`

### ideas.explore_policy
- `policy_id`: `text` [PK]
- `explore_floor`: `integer`
- `per_domain_min`: `integer`
- `version`: `text`
- `updated_at`: `timestamp with time zone`

### ideas.feed_snapshot_cards
- `snapshot_id`: `text` [PK]
- `section_id`: `text`
- `position`: `integer`
- `source_key`: `text`
- `origin`: `text`
- `display_title`: `text` [null]
- `display_summary`: `text` [null]
- `context_label`: `text` [null]
- `detail_description`: `text` [null]
- `build_summary`: `text` [null]
- `type_label`: `text` [null]
- `prerequisite_notes`: `text` [null]

### ideas.feed_snapshot_sections
- `snapshot_id`: `text` [PK, FK→ideas.feed_snapshots]
- `section_id`: `text`
- `position`: `integer`
- `role`: `text`
- `title`: `text`
- `subtitle`: `text` [null]

### ideas.feed_snapshots
- `snapshot_id`: `text` [PK]
- `generated_at`: `timestamp with time zone`
- `is_active`: `boolean`

### ideas.icon_embeddings
- `icon_key`: `text` [PK]
- `model_name`: `text`
- `descriptions_digest`: `text`
- `embedding`: `vector(384)`
- `created_at`: `timestamp with time zone`

### ideas.idea_anchors
- `idea_id`: `text` [PK, FK→ideas.ideas]
- `anchor_kind`: `text`
- `anchor_id`: `text`
- `lifecycle_status`: `text`
- `build_status`: `text` [null]
- `surfaced_at`: `timestamp with time zone` [null]
- `impression_count`: `integer`
- `decided_at`: `timestamp with time zone` [null]
- `dismiss_reason`: `text` [null]
- `created_at`: `timestamp with time zone`
- `updated_at`: `timestamp with time zone`

### ideas.idea_build_status
- `source_namespace`: `text` [PK]
- `source_kind`: `text`
- `source_id`: `text`
- `status`: `text`
- `updated_at`: `timestamp with time zone`
- `selected_item_ids`: `text[]` [null]

### ideas.idea_card_feeds
- `feed_id`: `text` [PK]
- `source`: `text`
- `payload_json`: `jsonb`
- `generated_at`: `timestamp with time zone`
- `updated_at`: `timestamp with time zone`

### ideas.idea_dedup
- `idea_id`: `text` [PK, FK→ideas.ideas]
- `content_fingerprint`: `text`
- `embedding_ref`: `text` [null]
- `canonical_idea_id`: `text` [FK→ideas.ideas, null]
- `merged_at`: `timestamp with time zone` [null]
- `merge_reason`: `text` [null]
- `created_at`: `timestamp with time zone`

### ideas.idea_events
- `event_id`: `text` [PK]
- `idea_id`: `text` [FK→ideas.ideas]
- `event_type`: `text`
- `dismissal_reason`: `text` [null]
- `anchor_kind`: `text` [null]
- `anchor_id`: `text` [null]
- `lane`: `text` [null]
- `request_id`: `text`
- `metadata`: `jsonb`
- `created_at`: `timestamp with time zone`

### ideas.idea_feedback_state
- `idea_id`: `text` [PK, FK→ideas.ideas]
- `feedback`: `text`
- `reason`: `text` [null]
- `event_id`: `text`
- `surface`: `text` [null]
- `created_at`: `timestamp with time zone`
- `updated_at`: `timestamp with time zone`

### ideas.idea_install_assets
- `asset_id`: `text` [PK]
- `idea_id`: `text` [FK→ideas.ideas]
- `asset_type`: `text`
- `asset_json`: `jsonb`
- `created_at`: `timestamp with time zone`
- `updated_at`: `timestamp with time zone`

### ideas.idea_items
- `idea_item_id`: `text` [PK]
- `idea_id`: `text` [FK→ideas.ideas]
- `position`: `integer`
- `kind`: `text`
- `title`: `text`
- `summary`: `text`
- `detail_description`: `text` [null]
- `instructions`: `text` [null]
- `build_plan_markdown`: `text` [null]
- `status`: `text` [null]
- `created_at`: `timestamp with time zone`
- `updated_at`: `timestamp with time zone`

### ideas.idea_quality
- `idea_id`: `text` [PK, FK→ideas.ideas]
- `relevance_score`: `double precision`
- `novelty_score`: `double precision`
- `feasibility_score`: `double precision`
- `composite_score`: `double precision`
- `composite_version`: `text`
- `scorer`: `text`
- `decision_outcome`: `text`
- `decision_reasons`: `jsonb`
- `scored_at`: `timestamp with time zone`
- `value_score`: `double precision`

### ideas.idea_sources
- `source_namespace`: `text` [PK]
- `source_kind`: `text`
- `source_id`: `text`
- `idea_id`: `text` [FK→ideas.ideas]
- `position`: `integer` [null]
- `metadata_json`: `jsonb`

### ideas.idea_tags
- `idea_id`: `text` [PK, FK→ideas.ideas]
- `position`: `integer`
- `tag`: `text`

### ideas.ideas
- `idea_id`: `text` [PK]
- `kind`: `text`
- `title`: `text`
- `summary`: `text`
- `rationale`: `text` [null]
- `category_label`: `text` [null]
- `date_label`: `text` [null]
- `audience`: `text` [null]
- `lane`: `text` [null]
- `install_markdown`: `text` [null]
- `created_at`: `timestamp with time zone`
- `updated_at`: `timestamp with time zone`
- `domain`: `text`
- `dedup_key`: `text` [null]
- `expires_at`: `timestamp with time zone` [null]
- `generator`: `text` [null]
- `search_vector`: `tsvector` [null]
- `prerequisite_notes`: `text` [null]
- `build_summary`: `text` [null]
- `category_index`: `bigint` [null]
- `embedding`: `vector(384)` [null]

## ingest — 1 tables

### ingest.data_source_events
- `event_id`: `bigint` [PK]
- `global_seq`: `bigint`
- `ingest_id`: `text`
- `producer_id`: `text`
- `source`: `text`
- `origin`: `text`
- `processing_lane`: `text`
- `received_at_text`: `text`
- `received_at_unix_ms`: `bigint`
- `payload_representation`: `text`
- `payload_sha256`: `text` [null]
- `status`: `text`
- `received_at`: `timestamp with time zone`
- `processed_at_text`: `text` [null]
- `processed_at_unix_ms`: `bigint` [null]
- `processed_at`: `timestamp with time zone` [null]
- `handoff_message_id`: `text` [null]
- `summary_preview`: `text` [null]
- `failure_code`: `text` [null]
- `failure_message`: `text` [null]
- `payload`: `text`
- `presentation_locale`: `text` [null]

## media — 4 tables

### media.descriptions
- `media_id`: `text` [PK]
- `model`: `text` [null]
- `version`: `bigint`
- `created_at`: `timestamp with time zone`
- `description_text`: `text`
- `summary_short_text`: `text`
- `summary_full_text`: `text` [null]
- `people_text`: `text` [null]
- `activity_text`: `text` [null]
- `objects_text`: `text` [null]
- `ocr_text`: `text` [null]
- `location_hint_text`: `text` [null]

### media.exif_values
- `exif_value_id`: `bigint` [PK]
- `media_id`: `text`
- `tag_name`: `text`
- `scalar_type`: `text`
- `scalar_value`: `text`

### media.items
- `media_id`: `text` [PK]
- `source`: `text`
- `source_media_id`: `text`
- `node_id`: `text` [null]
- `file_uri`: `text`
- `media_type`: `text`
- `sha256`: `bytea` [null]
- `byte_len`: `bigint` [null]
- `taken_at`: `timestamp with time zone` [null]
- `taken_at_local`: `text` [null]
- `taken_at_local_date`: `date` [null]
- `uploaded_at`: `timestamp with time zone`
- `uploaded_at_unix`: `bigint`
- `local_identifier`: `text` [null]
- `description_status`: `text`
- `description_attempt_count`: `integer`
- `description_next_retry_at_unix`: `bigint` [null]

### media.locations
- `media_id`: `text` [PK]
- `latitude`: `double precision` [null]
- `longitude`: `double precision` [null]
- `altitude_meters`: `double precision` [null]
- `location_source`: `text` [null]
- `geocode_attempt_count`: `integer`
- `geocode_next_retry_at_unix`: `bigint` [null]
- `location_text`: `text` [null]

## memory — 6 tables

### memory.claims
- `claim_id`: `text` [PK]
- `run_id`: `text`
- `kind`: `text`
- `salience`: `text`
- `claim_text`: `text`
- `quote`: `text` [null]
- `speaker`: `text`
- `evidence_handles`: `jsonb`
- `supersedes_claim_id`: `text` [null]
- `status`: `text`
- `confidence`: `double precision`
- `first_seen`: `timestamp with time zone`
- `reinforced_at`: `timestamp with time zone`
- `valid_until`: `timestamp with time zone` [null]
- `source_path`: `text`
- `source_line`: `bigint`
- `created_at`: `timestamp with time zone`
- `updated_at`: `timestamp with time zone`

### memory.embedding_models
- `embedding_model_id`: `bigint` [PK]
- `model_name`: `text`
- `dimensions`: `integer`
- `distance_metric`: `text`
- `created_at`: `timestamp with time zone`

### memory.embeddings
- `memory_embedding_id`: `bigint` [PK]
- `memory_entry_id`: `bigint` [FK→memory.entries]
- `embedding_model_id`: `bigint` [FK→memory.embedding_models]
- `embedding`: `vector(384)`
- `created_at`: `timestamp with time zone`

### memory.entries
- `memory_entry_id`: `bigint` [PK]
- `memory_uri`: `text`
- `chunk_id`: `text`
- `source_type`: `text`
- `status`: `text`
- `privacy_class`: `text`
- `confidence`: `double precision`
- `citation_path`: `text` [null]
- `line_start`: `bigint`
- `line_end`: `bigint`
- `created_at`: `timestamp with time zone`
- `created_at_unix`: `bigint`
- `title_text`: `text` [null]
- `body_text`: `text`
- `reason_text`: `text` [null]

### memory.entry_attributes
- `memory_entry_attribute_id`: `bigint` [PK]
- `memory_entry_id`: `bigint` [FK→memory.entries]
- `attribute_name`: `text`
- `scalar_value`: `text`

### memory.metadata
- `key`: `text` [PK]
- `value`: `text`
- `updated_at`: `timestamp with time zone`

## messages — 1 tables

### messages.native
- `message_id`: `bigint` [PK]
- `node_id`: `text`
- `external_id`: `text`
- `thread_id`: `text` [null]
- `address`: `text` [null]
- `contact_name`: `text` [null]
- `body`: `text` [null]
- `direction`: `text` [null]
- `kind`: `text` [null]
- `is_read`: `boolean` [null]
- `sent_at_unix_ms`: `bigint` [null]
- `created_at`: `timestamp with time zone`

## podcasts — 3 tables

### podcasts.episodes
- `slug`: `text` [PK]
- `title`: `text`
- `description`: `text`
- `created_at`: `text`
- `duration_secs`: `bigint`
- `audio_path`: `text`
- `chunk_count`: `bigint`
- `cover_path`: `text` [null]
- `episode_url`: `text` [null]
- `feed_url`: `text` [null]
- `updated_at`: `timestamp with time zone`
- `spotify_episode_uri`: `text` [null]
- `script`: `text` [null]
- `topics`: `text` [null]

### podcasts.feed
- `singleton`: `boolean` [PK]
- `feed_title`: `text`
- `feed_id`: `text` [null]
- `feed_url`: `text` [null]
- `cover_path`: `text` [null]
- `updated_at`: `timestamp with time zone`
- `spotify_show_url`: `text` [null]
- `spotify_show_id`: `text` [null]

### podcasts.feeds
- `feed_id`: `text` [PK]
- `feed_title`: `text`
- `feed_url`: `text` [null]
- `cover_path`: `text` [null]
- `spotify_show_url`: `text` [null]
- `spotify_show_id`: `text` [null]
- `created_at`: `timestamp with time zone`
- `updated_at`: `timestamp with time zone`

## runtime — 41 tables

### runtime.agent_todo_snapshots
- `conversation_id`: `text` [PK]
- `agent_identity`: `text`
- `items_json`: `text`
- `revision`: `bigint`
- `updated_at_ms`: `bigint`

### runtime.avatar_state
- `singleton`: `boolean` [PK]
- `active_stem`: `text`
- `image_path`: `text`
- `darkmode_image_path`: `text` [null]
- `chat_theme_color`: `text` [null]
- `chat_theme_color_override`: `text` [null]
- `static_frame_paths`: `text[]`
- `video_variants_json`: `text`
- `darkmode_video_variants_json`: `text`
- `video_variant_repair_json`: `text`
- `darkmode_video_variant_repair_json`: `text`
- `revision`: `bigint`
- `updated_at_ms`: `bigint`
- `image_variant_assets_json`: `text`
- `avatar_asset_repair_json`: `text`
- `choreography_profile_json`: `text` [null]
- `finalization_id`: `text` [null]
- `avatar_milestones_json`: `text`
- `legacy_folded_at_ms`: `bigint` [null]

### runtime.browser_tasks
- `task_id`: `text` [PK]
- `browser_session_id`: `text`
- `root_session_id`: `text`
- `owner_agent_id`: `text` [null]
- `root_message_id`: `text`
- `stream_owner_message_id`: `text`
- `status`: `text`
- `title`: `text`
- `step_count`: `integer`
- `channel_context_json`: `text` [null]
- `latest_action_id`: `text` [null]
- `latest_tab_json`: `text` [null]
- `latest_screenshot_json`: `text` [null]
- `created_at`: `timestamp with time zone`
- `updated_at`: `timestamp with time zone`
- `completed_at`: `timestamp with time zone` [null]
- `terminal_reason`: `text` [null]
- `admission_seq`: `bigint` [null]
- `presentation_root_session_id`: `text` [null]
- `parent_agent_id`: `text` [null]
- `tool_call_id`: `text` [null]
- `request_trace_context_json`: `text` [null]
- `requester_source`: `text` [null]
- `requester_channel`: `text` [null]
- `requester_model`: `text` [null]
- `requester_effective_model`: `text` [null]
- `request_mode_json`: `text` [null]
- `max_training_tier_json`: `text` [null]
- `is_task_card_visible`: `boolean`
- `initial_instruction`: `text` [null]
- `history_source_agent_id`: `text` [null]
- `state_revision`: `bigint`
- `deadline_at`: `timestamp with time zone` [null]
- `continuation_root_task_id`: `text` [null]
- `terminal_interrupt_subtype`: `text` [null]
- `outcome_status`: `text` [null]
- `outcome_reason`: `text` [null]
- `outcome_at`: `timestamp with time zone` [null]
- `retention_end_reason`: `text` [null]
- `card_generation`: `bigint`
- `input_grants_json`: `text`
- `browser_navigation_attempted`: `boolean`
- `terminal_user_update_due_at`: `timestamp with time zone` [null]
- `egress_profile`: `text` [null]
- `run_number`: `bigint` [null]
- `run_started_at`: `timestamp with time zone` [null]
- `run_presentation_locale`: `text` [null]
- `run_location_context_json`: `text` [null]
- `owner_kind`: `text` [null]
- `broker_instance`: `text`

### runtime.channel_deliveries
- `delivery_key`: `text` [PK]
- `delivery_kind`: `text`
- `surface`: `text`
- `target`: `text` [null]
- `payload_json`: `text`
- `state`: `text`
- `dispatch_boot_generation`: `text` [null]
- `attempt_count`: `integer`
- `next_attempt_at_utc`: `bigint`
- `result_json`: `text` [null]
- `last_error`: `text` [null]
- `created_at_utc`: `bigint`
- `updated_at_utc`: `bigint`
- `terminal_at_utc`: `bigint` [null]

### runtime.channel_message_bindings
- `binding_id`: `bigint` [PK]
- `channel_message_id`: `text` [null]
- `channel`: `text` [null]
- `jarvis_message_id`: `text`
- `provider`: `text` [null]
- `provider_channel_id`: `text` [null]
- `provider_message_id`: `text` [null]
- `created_at_ms`: `bigint` [null]
- `created_at`: `timestamp with time zone`

### runtime.chat_event_derived_write_backlog
- `event_seq`: `bigint` [PK, FK→runtime.events]
- `resources_json`: `text`
- `attempts`: `integer`
- `last_error`: `text` [null]
- `created_at`: `timestamp with time zone`
- `updated_at`: `timestamp with time zone`
- `deadlettered_at`: `timestamp with time zone` [null]

### runtime.checkout_spend_checkpoints
- `operation_id`: `text` [PK, FK→runtime.checkout_spend_operations]
- `provider`: `text`
- `checkpoint_key`: `text`
- `external_id`: `text` [null]
- `detail`: `jsonb` [null]
- `claimed_at_ms`: `bigint`
- `bound_at_ms`: `bigint` [null]

### runtime.checkout_spend_operations
- `operation_id`: `text` [PK]
- `client_task_key`: `text`
- `checkout_request_id`: `text` [null]
- `wallet_request_id`: `text` [null]
- `approval_id`: `text` [null]
- `state`: `text`
- `expires_at_ms`: `bigint`
- `created_at_ms`: `bigint`
- `updated_at_ms`: `bigint`
- `terminal_result`: `jsonb` [null]
- `recovery_epoch`: `smallint`
- `wallet_provider`: `text`

### runtime.client_rendering_capabilities
- `singleton`: `boolean` [PK]
- `client_id`: `text`
- `platform`: `text`
- `supported_presentations`: `text[]`
- `supported_inline_presentations`: `text[]`
- `declared_at`: `timestamp with time zone`

### runtime.context_snapshots
- `snapshot_kind`: `text` [PK]
- `owner_key`: `text`
- `version`: `bigint`
- `source_watermark`: `text` [null]
- `fingerprint`: `text` [null]
- `freshness_class`: `text`
- `state_json`: `text`
- `refreshed_at`: `timestamp with time zone`
- `invalidated_at`: `timestamp with time zone` [null]
- `stale_after`: `timestamp with time zone` [null]
- `last_error`: `text` [null]
- `updated_at`: `timestamp with time zone`

### runtime.dev_notice_watermark
- `singleton`: `boolean` [PK]
- `last_seen_version_id`: `bigint`

### runtime.event_channels
- `channel`: `text` [PK]
- `event_seq`: `bigint` [FK→runtime.events]

### runtime.event_hook_space_owners
- `hook_id`: `text` [PK]
- `space_slug`: `text`
- `created_at`: `timestamp with time zone`
- `updated_at`: `timestamp with time zone`

### runtime.event_payload_fields
- `event_payload_field_id`: `bigint` [PK]
- `event_seq`: `bigint` [FK→runtime.events]
- `field_path`: `text`
- `scalar_type`: `text`
- `scalar_value`: `text` [null]
- `text_value`: `text` [null]

### runtime.events
- `event_seq`: `bigint` [PK]
- `event_id`: `uuid`
- `event_kind`: `runtime.event_kind`
- `event_name`: `text`
- `request_id`: `text` [FK→runtime.requests, null]
- `root_request_id`: `text` [FK→runtime.requests, null]
- `parent_request_id`: `text` [FK→runtime.requests, null]
- `transcript_surface`: `runtime.transcript_surface`
- `visibility`: `runtime.visibility`
- `source`: `text`
- `role`: `runtime.message_role` [null]
- `chat_kind`: `text`
- `stream_lane`: `text`
- `message_id`: `text` [null]
- `parent_message_id`: `text` [null]
- `reply_to_message_id`: `text` [null]
- `reply_target_message_id`: `text` [null]
- `parent_agent_id`: `text` [null]
- `agent_id`: `text` [null]
- `idempotency_key`: `text` [null]
- `display_text_ready`: `boolean`
- `created_at`: `timestamp with time zone`
- `payload_json`: `text` [null]
- `channel_context_json`: `text` [null]

### runtime.execute_resolve_runs
- `run_id`: `text` [PK]
- `worker_kind`: `text`
- `source_ref`: `text`
- `lane_key`: `text`
- `status`: `text`
- `terminal_decision`: `text` [null]
- `terminal_message`: `text` [null]
- `handoff_message_id`: `text` [null]
- `last_error_stage`: `text` [null]
- `last_error_message`: `text` [null]
- `metadata_json`: `jsonb`
- `created_at`: `timestamp with time zone`
- `updated_at`: `timestamp with time zone`
- `worker_generation`: `bigint`
- `worker_phase`: `text` [null]
- `worker_agent_id`: `text` [null]
- `worker_started_at`: `timestamp with time zone` [null]
- `execute_message_id`: `text` [null]
- `resolve_message_id`: `text` [null]

### runtime.idea_execution_pending
- `activation_id`: `text` [PK]
- `root_submission_message_id`: `text`
- `session_id`: `text`
- `idea_card_id`: `text`
- `idea_card_kind`: `text`
- `dispatched_at`: `timestamp with time zone`
- `reconciled_at`: `timestamp with time zone` [null]

### runtime.invite_badge_seen_state
- `singleton`: `boolean` [PK]
- `last_seen_badge_version`: `bigint`

### runtime.maintenance_markers
- `marker_key`: `text` [PK]
- `completed_at`: `timestamp with time zone`
- `detail_json`: `jsonb`

### runtime.message_attachments
- `attachment_id`: `bigint` [PK]
- `message_id`: `text` [FK→runtime.messages]
- `ordinal`: `integer`
- `attachment_kind`: `text`
- `file_uri`: `text`
- `media_type`: `text` [null]
- `byte_len`: `bigint` [null]
- `sha256`: `bytea` [null]
- `caption_text`: `text` [null]
- `transcription_text`: `text` [null]

### runtime.message_reactions
- `message_id`: `text` [PK, FK→runtime.messages]
- `reaction_emoji`: `text` [null]
- `updated_at`: `timestamp with time zone`

### runtime.messages
- `message_id`: `text` [PK]
- `event_seq`: `bigint` [FK→runtime.events]
- `role`: `runtime.message_role`
- `prompt_rendering_id`: `bigint` [null]
- `author_label`: `text` [null]
- `provider_message_id`: `text` [null]
- `created_at`: `timestamp with time zone`
- `body`: `text` [null]

### runtime.product_improvements_preference
- `singleton`: `boolean` [PK]
- `enabled`: `boolean`
- `updated_at`: `timestamp with time zone`

### runtime.raw_signal_collections
- `collection_id`: `uuid` [PK]
- `tool`: `text`
- `mode`: `text`
- `fetched_at`: `timestamp with time zone`
- `partial`: `boolean`
- `skipped`: `boolean`
- `per_request_timeout_secs`: `integer`
- `max_total_secs`: `integer`
- `source_names`: `text[]`
- `entry_count`: `integer`
- `metadata_json`: `jsonb`
- `created_at`: `timestamp with time zone`

### runtime.raw_signal_entries
- `entry_id`: `bigint` [PK]
- `collection_id`: `uuid` [FK→runtime.raw_signal_collections]
- `ordinal`: `integer`
- `source`: `text`
- `name`: `text`
- `method`: `text`
- `logical_url`: `text`
- `ok`: `boolean`
- `status`: `integer` [null]
- `logical_path`: `text` [null]
- `error`: `text` [null]
- `duration_ms`: `bigint`
- `bytes`: `bigint` [null]
- `body_json`: `jsonb` [null]
- `created_at`: `timestamp with time zone`

### runtime.requests
- `request_id`: `text` [PK]
- `root_request_id`: `text` [FK→runtime.requests, null]
- `parent_request_id`: `text` [FK→runtime.requests, null]
- `request_origin`: `text`
- `transcript_surface`: `runtime.transcript_surface`
- `root_work_class`: `text`
- `created_at`: `timestamp with time zone`

### runtime.resources
- `resource_id`: `bigint` [PK]
- `event_seq`: `bigint` [FK→runtime.events]
- `ordinal`: `integer`
- `resource_kind`: `text`
- `resource_key`: `text`
- `label`: `text` [null]
- `mime_type`: `text` [null]
- `size_bytes`: `bigint` [null]
- `metadata_json`: `text` [null]
- `created_at`: `timestamp with time zone`

### runtime.search_documents
- `search_document_id`: `bigint` [PK]
- `owner_table`: `text`
- `owner_key`: `text`
- `language`: `regconfig`
- `search_vector`: `tsvector`
- `updated_at`: `timestamp with time zone`
- `search_text`: `text`

### runtime.skill_invalidation_state
- `skill_name`: `text` [PK]
- `used`: `boolean`
- `pending_invalidation_hash`: `text` [null]
- `pending_manifest_rel_path`: `text` [null]
- `last_delivered_invalidation_hash`: `text` [null]

### runtime.stripe_link_spend_requests
- `approval_id`: `text` [PK]
- `continuation_root_task_id`: `text`
- `merchant_origin`: `text`
- `checkout_metadata`: `jsonb`
- `approved_amount_minor`: `bigint`
- `approved_currency`: `text`
- `lifecycle_state`: `text`
- `create_attempt_id`: `uuid`
- `stripe_spend_request_id`: `text` [null]
- `expires_at`: `timestamp with time zone`
- `assigned_browser_task_id`: `text` [null]
- `assigned_at`: `timestamp with time zone` [null]
- `cancel_attempt_id`: `uuid` [null]
- `closed_at`: `timestamp with time zone` [null]
- `close_reason`: `text` [null]
- `created_at`: `timestamp with time zone`
- `owner_browser_task_id`: `text` [null]
- `claiming_browser_task_lineage_id`: `text` [null]
- `cancellation_confirmation_task_state_revision`: `bigint` [null]
- `cancellation_confirmation_response_message_id`: `text` [null]
- `wallet_provider`: `text`

### runtime.summaries
- `id`: `bigint` [PK]
- `summary_key`: `text` [null]
- `summary_text`: `text` [null]
- `created_at_ms`: `bigint`

### runtime.tool_calls
- `tool_call_id`: `bigint` [PK]
- `event_seq`: `bigint` [FK→runtime.events]
- `call_id`: `text`
- `tool_name`: `text`
- `server_name`: `text` [null]
- `status`: `text`
- `created_at`: `timestamp with time zone`
- `arguments_json`: `text` [null]

### runtime.tool_outputs
- `tool_output_id`: `bigint` [PK]
- `event_seq`: `bigint` [FK→runtime.events]
- `call_id`: `text`
- `status`: `text`
- `created_at`: `timestamp with time zone`
- `output_text`: `text` [null]
- `error_text`: `text` [null]

### runtime.widgets
- `widget_id`: `text` [PK]
- `kind`: `text`
- `data_json`: `text`
- `display_text`: `text` [null]
- `state_bundle_json`: `text`
- `state_version`: `bigint`
- `state_updated_at_ms`: `bigint` [null]
- `created_at_ms`: `bigint`
- `updated_at_ms`: `bigint`

### runtime.work_items
- `work_id`: `text` [PK]
- `root_work_id`: `text` [FK→runtime.work_items, null]
- `parent_work_id`: `text` [FK→runtime.work_items, null]
- `request_id`: `text` [FK→runtime.requests, null]
- `root_session_id`: `text` [null]
- `session_id`: `text` [null]
- `agent_id`: `text` [null]
- `root_message_id`: `text` [null]
- `stream_owner_message_id`: `text` [null]
- `source`: `text` [null]
- `request_origin`: `text` [null]
- `work_class`: `text`
- `transcript_surface`: `text` [null]
- `state`: `text`
- `phase`: `text` [null]
- `subphase`: `text` [null]
- `priority`: `integer`
- `preemptibility`: `text`
- `deadline_at`: `timestamp with time zone` [null]
- `heartbeat_at`: `timestamp with time zone`
- `lease_owner`: `text` [null]
- `retry_policy_json`: `text` [null]
- `metadata_json`: `text`
- `terminal_reason`: `text` [null]
- `terminal_detail`: `text` [null]
- `diagnostic_json`: `text` [null]
- `created_at`: `timestamp with time zone`
- `updated_at`: `timestamp with time zone`
- `terminalized_at`: `timestamp with time zone` [null]

### runtime.workflow_agent_calls
- `call_id`: `text` [PK]
- `run_id`: `text` [FK→runtime.workflow_runs]
- `phase_run_id`: `text` [FK→runtime.workflow_phase_runs, null]
- `replay_key`: `text`
- `cache_key`: `text`
- `call_ordinal`: `integer`
- `prompt`: `text`
- `options_json`: `jsonb`
- `child_agent_id`: `text` [null]
- `status`: `text`
- `cached_from_call_id`: `text` [FK→runtime.workflow_agent_calls, null]
- `final_response`: `text` [null]
- `input_tokens`: `bigint`
- `output_tokens`: `bigint`
- `tool_call_count`: `bigint`
- `duration_ms`: `bigint` [null]
- `created_at`: `timestamp with time zone`
- `started_at`: `timestamp with time zone` [null]
- `updated_at`: `timestamp with time zone`
- `completed_at`: `timestamp with time zone` [null]
- `error`: `text` [null]
- `child_message_id`: `text` [null]

### runtime.workflow_launch_occurrence_aliases
- `launch_occurrence_agent_id`: `text` [PK]
- `launch_occurrence_message_id`: `text`
- `launch_occurrence_tool_call_id`: `text`
- `run_id`: `text` [FK→runtime.workflow_runs]
- `contract_fingerprint`: `text`
- `created_at`: `timestamp with time zone`

### runtime.workflow_legacy_launch_occurrence_blocks
- `launch_occurrence_agent_id`: `text` [PK]
- `launch_occurrence_message_id`: `text`
- `launch_occurrence_tool_call_id`: `text`
- `legacy_run_id`: `text` [FK→runtime.workflow_runs]
- `reason`: `text`
- `created_at`: `timestamp with time zone`

### runtime.workflow_phase_runs
- `phase_run_id`: `text` [PK]
- `run_id`: `text` [FK→runtime.workflow_runs]
- `phase_name`: `text`
- `ordinal`: `integer`
- `status`: `text`
- `agent_total`: `integer`
- `agent_completed`: `integer`
- `input_tokens`: `bigint`
- `output_tokens`: `bigint`
- `started_at`: `timestamp with time zone` [null]
- `updated_at`: `timestamp with time zone`
- `completed_at`: `timestamp with time zone` [null]
- `error`: `text` [null]

### runtime.workflow_runs
- `run_id`: `text` [PK]
- `root_agent_id`: `text`
- `parent_message_id`: `text` [null]
- `launching_agent_id`: `text` [null]
- `launching_message_id`: `text` [null]
- `launching_tool_call_id`: `text` [null]
- `task_id`: `text`
- `resume_from_run_id`: `text` [FK→runtime.workflow_runs, null]
- `workflow_name`: `text`
- `description`: `text`
- `phases_json`: `jsonb`
- `args_json`: `jsonb` [null]
- `workspace_root`: `text`
- `script_path`: `text`
- `script_sha256`: `text`
- `request_trace_json`: `jsonb` [null]
- `executor_id`: `text` [null]
- `execution_attempt`: `integer`
- `heartbeat_at`: `timestamp with time zone`
- `lease_expires_at`: `timestamp with time zone` [null]
- `recovery_count`: `integer`
- `last_recovery_reason`: `text` [null]
- `last_recovered_at`: `timestamp with time zone` [null]
- `status`: `text`
- `error`: `text` [null]
- `final_result`: `text` [null]
- `created_at`: `timestamp with time zone`
- `started_at`: `timestamp with time zone` [null]
- `updated_at`: `timestamp with time zone`
- `completed_at`: `timestamp with time zone` [null]
- `launch_mode`: `text`
- `terminal_handoff_channel`: `text` [null]
- `terminal_handoff_delivery_target`: `text` [null]
- `terminal_handoff_channel_context_json`: `jsonb` [null]
- `terminal_handoff_message_id`: `text` [null]
- `terminal_handoff_delivered_at`: `timestamp with time zone` [null]
- `terminal_handoff_attempt_count`: `integer`
- `terminal_handoff_last_attempt_at`: `timestamp with time zone` [null]
- `terminal_handoff_next_attempt_at`: `timestamp with time zone` [null]
- `terminal_handoff_last_error`: `text` [null]
- `launch_occurrence_agent_id`: `text` [null]
- `launch_occurrence_message_id`: `text` [null]
- `launch_occurrence_tool_call_id`: `text` [null]
- `sync_wait_deadline_at`: `timestamp with time zone` [null]
- `terminal_handoff_disposition`: `text` [null]
- `terminal_handoff_generation`: `integer`
- `magi_workload_class`: `text` [null]

### runtime.writer_epoch
- `singleton`: `boolean` [PK]
- `rv_epoch`: `bigint`
- `updated_at`: `timestamp with time zone`

## scheduler — 12 tables

### scheduler.cron_mutations
- `mutation_seq`: `bigint` [PK]
- `job_id`: `text`
- `action`: `text`
- `occurred_at_ms`: `bigint`
- `carrier_message_id`: `text`
- `request_id`: `text`
- `tool_call_id`: `text`
- `input_message_ids`: `text[]`
- `created_at`: `timestamp with time zone`

### scheduler.delivery_outbox
- `delivery_key`: `text` [PK]
- `job_id`: `text`
- `run_id`: `text`
- `payload_json`: `text`
- `state`: `text`
- `dispatch_boot_generation`: `text` [null]
- `attempt_count`: `integer`
- `next_attempt_at_utc`: `bigint`
- `last_error`: `text` [null]
- `created_at_utc`: `bigint`
- `updated_at_utc`: `bigint`
- `delivered_at_utc`: `bigint` [null]

### scheduler.doctor_run_plans
- `run_id`: `text` [PK, FK→scheduler.job_runs]
- `tasks_json`: `jsonb`
- `created_at`: `timestamp with time zone`

### scheduler.doctor_task_state
- `task_name`: `text` [PK]
- `evidence_hash`: `text`
- `last_success_run_id`: `text`
- `last_success_scheduled_for_utc`: `bigint`
- `updated_at`: `timestamp with time zone`

### scheduler.events
- `scheduler_event_id`: `bigint` [PK]
- `job_id`: `text` [null]
- `run_id`: `text` [null]
- `event_name`: `text`
- `detail`: `text` [null]
- `created_at`: `timestamp with time zone`

### scheduler.job_definitions
- `job_definition_id`: `bigint` [PK]
- `job_id`: `text` [FK→scheduler.jobs]
- `version`: `integer`
- `prompt_template_id`: `bigint` [null]
- `created_at`: `timestamp with time zone`
- `retired_at`: `timestamp with time zone` [null]
- `task_text`: `text`

### scheduler.job_idea_scope
- `job_id`: `text` [PK, FK→scheduler.jobs]
- `idea_provenance_json`: `text`
- `created_at`: `timestamp with time zone`
- `updated_at`: `timestamp with time zone`

### scheduler.job_runs
- `run_id`: `text` [PK]
- `job_id`: `text` [FK→scheduler.jobs]
- `job_definition_id`: `bigint` [FK→scheduler.job_definitions, null]
- `request_id`: `text` [FK→runtime.requests, null]
- `scheduled_for`: `timestamp with time zone`
- `scheduled_for_utc`: `bigint`
- `trigger_reason`: `text` [null]
- `started_at`: `timestamp with time zone` [null]
- `started_at_utc`: `bigint` [null]
- `finished_at`: `timestamp with time zone` [null]
- `finished_at_utc`: `bigint` [null]
- `status`: `scheduler.run_status`
- `result_summary`: `text` [null]
- `error_text`: `text` [null]
- `attempt`: `integer`
- `worker_phase`: `text` [null]
- `worker_phase_attempt`: `integer` [null]
- `worker_agent_id`: `text` [null]
- `worker_boot_generation`: `text` [null]
- `worker_claimed_at_utc`: `bigint` [null]
- `worker_execution_timed_out`: `boolean`
- `worker_history_agent_id`: `text` [null]
- `product_endpoint_deadline_at_ms`: `bigint` [null]
- `presentation_locale`: `text`
- `onboarding_tour_body`: `text` [null]
- `pending_terminal_result_json`: `text` [null]
- `worker_approval_wait_json`: `text` [null]

### scheduler.jobs
- `job_id`: `text` [PK]
- `is_heartbeat`: `boolean`
- `source_path`: `text` [null]
- `source_hash`: `text` [null]
- `schedule_kind`: `text`
- `enabled`: `boolean`
- `schedule_expr`: `text`
- `timezone`: `text`
- `retry_on_failure`: `boolean`
- `max_retries`: `integer`
- `delivery_targets_json`: `text`
- `next_run_at`: `timestamp with time zone` [null]
- `next_run_at_utc`: `bigint`
- `last_run_at_utc`: `bigint` [null]
- `last_success_at_utc`: `bigint` [null]
- `last_status`: `text` [null]
- `consecutive_failures`: `bigint`
- `updated_at_utc`: `bigint`
- `created_at`: `timestamp with time zone`
- `updated_at`: `timestamp with time zone`
- `blocked_reason`: `text` [null]
- `blocked_dependency`: `text` [null]
- `blocked_at_utc`: `bigint` [null]
- `next_blocked_probe_at_utc`: `bigint` [null]

### scheduler.scheduled_resume_registrations
- `resume_at_utc`: `bigint` [PK]
- `schedule_id`: `text` [null]
- `dispatch_at_ms`: `bigint` [null]
- `acknowledgement_attempts`: `integer`
- `created_at`: `timestamp with time zone`
- `job_name`: `text` [null]
- `shadow`: `boolean`
- `registration_attempts`: `integer`
- `acknowledgement_next_attempt_at_ms`: `bigint`
- `environment_id`: `text` [null]

### scheduler.scheduled_resume_state
- `singleton`: `boolean` [PK]
- `next_resume_at_utc`: `bigint` [null]
- `registered_dispatch_at_ms`: `bigint` [null]
- `projection_complete`: `boolean`
- `registration_resume_at_utc`: `bigint` [null]
- `registered_schedule_id`: `text` [null]
- `next_resume_job_name`: `text` [null]

### scheduler.terminal_signals
- `run_id`: `text` [PK]
- `signal_type`: `text`
- `message`: `text` [null]
- `producer_request_trace_json`: `text` [null]
- `created_at`: `timestamp with time zone`
- `created_at_utc`: `bigint`
- `producer_agent_id`: `text` [null]
- `producer_phase_attempt`: `integer` [null]
- `producer_boot_generation`: `text` [null]
- `blocked_reason`: `text` [null]
- `blocked_dependency`: `text` [null]

## self_improvement — 12 tables

### self_improvement.backfill_day_runs
- `backfill_day_run_id`: `text` [PK]
- `objective_id`: `text`
- `day`: `date`
- `run_id`: `text`
- `request_id`: `text`
- `replay_hash`: `text`
- `status`: `text`
- `diagnostics`: `jsonb`
- `started_at`: `timestamp with time zone` [null]
- `finished_at`: `timestamp with time zone` [null]
- `created_at`: `timestamp with time zone`
- `updated_at`: `timestamp with time zone`

### self_improvement.calculation_records
- `replay_id`: `text` [PK]
- `card_id`: `text`
- `objective_id`: `text`
- `config_hash`: `text`
- `code_version`: `text`
- `source_handles`: `jsonb`
- `normalized_inputs`: `jsonb`
- `formulas`: `jsonb`
- `outputs`: `jsonb`
- `created_at`: `timestamp with time zone`

### self_improvement.calibration_records
- `calibration_id`: `text` [PK]
- `card_id`: `text`
- `objective_id`: `text`
- `change_id`: `text`
- `classification`: `text`
- `window_start`: `timestamp with time zone` [null]
- `window_end`: `timestamp with time zone` [null]
- `source_handles`: `jsonb`
- `diagnostics`: `jsonb`
- `created_at`: `timestamp with time zone`

### self_improvement.connector_read_audit
- `audit_id`: `bigint` [PK]
- `connector_id`: `text` [null]
- `device_node_id`: `text` [null]
- `method_key`: `text`
- `purpose`: `text`
- `request_id`: `text`
- `request_origin`: `text`
- `sensitivity`: `text`
- `source_handle`: `text` [null]
- `metadata`: `jsonb`
- `created_at`: `timestamp with time zone`

### self_improvement.conversation_follow_up_attempts
- `occurrence`: `text` [PK]
- `delivery_submission_id`: `text`
- `selector_decision`: `text` [null]
- `selector_message`: `text` [null]
- `feedback_note`: `text` [null]
- `selected_at`: `timestamp with time zone` [null]
- `state`: `text`
- `disposition_reason`: `text` [null]
- `surfaced_at`: `timestamp with time zone` [null]
- `terminal_at`: `timestamp with time zone` [null]
- `created_at`: `timestamp with time zone`
- `updated_at`: `timestamp with time zone`
- `source_root_agent_id`: `text` [null]
- `selector_priority`: `smallint` [null]

### self_improvement.handoff_dedupe
- `objective_id`: `text` [PK]
- `content_hash`: `text`
- `run_id`: `text`
- `emitted_at`: `timestamp with time zone`

### self_improvement.learning_adoption_events
- `id`: `bigint` [PK]
- `learning_id`: `text`
- `objective_id`: `text`
- `run_id`: `text`
- `outcome`: `text`
- `detail`: `jsonb`
- `created_at`: `timestamp with time zone`

### self_improvement.leases
- `lease_key`: `text` [PK]
- `owner`: `text`
- `objective_id`: `text` [null]
- `run_id`: `text` [FK→self_improvement.runs, null]
- `acquired_at`: `timestamp with time zone`
- `expires_at`: `timestamp with time zone`
- `heartbeat_at`: `timestamp with time zone`
- `metadata`: `jsonb`

### self_improvement.objective_markers
- `objective_id`: `text` [PK]
- `marker`: `text`
- `set_at`: `timestamp with time zone`

### self_improvement.objective_state
- `objective_id`: `text` [PK]
- `current_file_hash`: `text` [null]
- `projection_hash`: `text` [null]
- `status`: `text`
- `last_run_utc`: `timestamp with time zone` [null]
- `state_json`: `jsonb`
- `updated_at`: `timestamp with time zone`

### self_improvement.relationship_briefs
- `brief_id`: `text` [PK]
- `title`: `text`
- `body_html`: `text`
- `anchored_idea_ids`: `jsonb`
- `request_id`: `text`
- `created_at`: `timestamp with time zone`
- `last_opened_at`: `timestamp with time zone` [null]

### self_improvement.runs
- `run_id`: `text` [PK]
- `objective_id`: `text`
- `state`: `text`
- `request_id`: `text`
- `request_origin`: `text`
- `phase`: `text` [null]
- `subphase`: `text` [null]
- `diagnostics`: `jsonb`
- `queued_at`: `timestamp with time zone`
- `started_at`: `timestamp with time zone` [null]
- `finished_at`: `timestamp with time zone` [null]
- `created_at`: `timestamp with time zone`
- `updated_at`: `timestamp with time zone`
- `run_clock`: `timestamp with time zone`
- `available_at`: `timestamp with time zone`
- `recovery_phase_version`: `integer` [null]
- `recovery_admitted_at`: `timestamp with time zone` [null]
- `recovery_admission_boot_id`: `text` [null]

## shell — 1 tables

### shell.user_state
- `source`: `text` [PK]
- `item_key`: `text`
- `is_favorite`: `boolean`
- `accessed_at_ms`: `bigint` [null]
- `frequency_score`: `double precision` [null]
- `favorite_order`: `double precision` [null]
- `display_name`: `text` [null]
- `icon`: `text` [null]
- `updated_at_ms`: `bigint`
- `last_opened_at_ms`: `bigint` [null]

## spaces — 9 tables

### spaces.action_arguments
- `action_argument_id`: `bigint` [PK]
- `invocation_id`: `text` [FK→spaces.action_invocations]
- `argument_name`: `text`
- `scalar_value`: `text` [null]
- `text_content`: `text` [null]

### spaces.action_invocations
- `global_seq`: `bigint`
- `invocation_id`: `text` [PK]
- `space_slug`: `text`
- `space_display_name`: `text` [null]
- `action`: `text`
- `transport`: `text`
- `request_id`: `text` [FK→runtime.requests, null]
- `status`: `text`
- `invoked_at_text`: `text` [null]
- `invoked_at_unix_ms`: `bigint`
- `invoked_at`: `timestamp with time zone`
- `settled_at_text`: `text` [null]
- `settled_at_unix_ms`: `bigint` [null]
- `duration_ms`: `bigint` [null]
- `args_preview`: `text` [null]
- `result_preview`: `text` [null]
- `error`: `text` [null]
- `stream_protocol_messages`: `bigint` [null]
- `stream_data_messages`: `bigint` [null]
- `finished_at`: `timestamp with time zone` [null]
- `source_kind`: `text` [null]
- `source_ref`: `text` [null]
- `trigger_ref`: `text` [null]

### spaces.action_results
- `invocation_id`: `text` [PK, FK→spaces.action_invocations]
- `result_text`: `text` [null]
- `error_text`: `text` [null]

### spaces.backfill_markers
- `marker`: `text` [PK]
- `completed_at`: `timestamp with time zone`

### spaces.file_artifact_identities
- `slug`: `text` [PK]
- `artifact_id`: `uuid`
- `created_at`: `timestamp with time zone`

### spaces.proposals
- `proposal_id`: `text` [PK]
- `root_session_id`: `text`
- `space_slug`: `text`
- `proposed_name`: `text`
- `params_json`: `text`
- `confirmed_at_text`: `text` [null]
- `confirmed_at`: `timestamp with time zone` [null]
- `created_at_text`: `text`
- `created_at`: `timestamp with time zone`

### spaces.shares
- `space_slug`: `text` [PK, FK→spaces.spaces]
- `share_type`: `text`
- `shortcode`: `text`
- `is_active`: `boolean`
- `share_id`: `text` [null]
- `cloudflare_deploy_status`: `text` [null]
- `cloudflare_public_url`: `text` [null]
- `cloudflare_actions_url`: `text` [null]
- `created_at`: `timestamp with time zone`
- `updated_at`: `timestamp with time zone`
- `cloudflare_storage_mode`: `text`
- `published_manifest_sha256`: `text` [null]

### spaces.spaces
- `space_slug`: `text` [PK]
- `display_name`: `text`
- `space_root_path`: `text` [null]
- `db_path`: `text` [null]
- `force_order`: `bigint` [null]
- `session_id`: `text` [null]
- `construction_status`: `text` [null]
- `construction_updated_at_text`: `text` [null]
- `created_at_text`: `text` [null]
- `updated_at_text`: `text` [null]
- `created_at`: `timestamp with time zone`
- `updated_at`: `timestamp with time zone`
- `archived_at`: `timestamp with time zone` [null]
- `current_build_id`: `text` [null]
- `created_from_proposal_id`: `text` [null]
- `space_id`: `uuid`
- `source`: `text`
- `shortcode`: `text` [null]
- `source_url`: `text` [null]
- `has_server_actions`: `boolean`
- `cloudflare_share_manifest_sha256`: `text` [null]
- `content_share_allowed`: `boolean` [null]
- `content_share_review_sha256`: `text` [null]
- `declared_capabilities_json`: `jsonb`
- `capability_manifest_revision`: `bigint`
- `builder_provenance_pending_build_id`: `text` [null]
- `builder_provenance_lost_build_id`: `text` [null]
- `builder_provenance_build_id`: `text` [null]
- `builder_provenance_evidence_jsonl`: `text` [null]
- `share_disclosure_review`: `jsonb` [null]
- `is_promoted`: `boolean`
- `artifact_audit_review`: `jsonb` [null]

### spaces.user_state
- `space_slug`: `text` [PK]
- `is_favorite`: `boolean`
- `accessed_at_ms`: `bigint` [null]
- `frequency_score`: `double precision` [null]
- `favorite_order`: `double precision` [null]
- `last_accessed_at`: `timestamp with time zone` [null]
- `pinned_at`: `timestamp with time zone` [null]
- `last_opened_at_ms`: `bigint` [null]
