# Steps to reproduce

1. Generate OAuth application

```rb
a = Doorkeeper::Application.create(name: 'Test Application', redirect_uri: 'urn:ietf:wg:oauth:2.0:oob')
p a.uid, a.plaintext_secret
```

1. Start Rails server

```sh
rails s
```

1. Use curl to check valid response for `GET /oauth/authorize` with invalid response type

```sh
curl -i -X GET -H "Content-Type: application/json" -H "Accept: application/json" "localhost:3000/oauth/authorize?response_type=test&client_id=DM7QY4XmmqgpvaH7h2ewLggiIwrrfm2EDBtKSgE2iIc&redirect_uri=urn:ietf:wg:oauth:2.0:oob&scope=openid"
```

Status code is `400 Bad Request`

```json
{"error":"unsupported_response_type","error_description":"The authorization server does not support this response type."}
```

1. Use curl to check invalid response for `DELETE /oauth/authorize` with invalid response type

```sh
curl -i -X DELETE -H "Content-Type: application/json" -H "Accept: application/json" "localhost:3000/oauth/authorize?response_type=test&client_id=DM7QY4XmmqgpvaH7h2ewLggiIwrrfm2EDBtKSgE2iIc&redirect_uri=urn:ietf:wg:oauth:2.0:oob&scope=openid"
```

Status code is `500 Internal Server Error`

Rails stacktrace:

```txt
Doorkeeper::Errors::InvalidTokenStrategy (Doorkeeper::Errors::InvalidTokenStrategy):

doorkeeper (5.6.4) lib/doorkeeper/request.rb:69:in `rescue in build_fallback_strategy_class'
doorkeeper (5.6.4) lib/doorkeeper/request.rb:57:in `build_fallback_strategy_class'
doorkeeper (5.6.4) lib/doorkeeper/request.rb:16:in `authorization_strategy'
doorkeeper (5.6.4) lib/doorkeeper/server.rb:12:in `authorization_request'
doorkeeper (5.6.4) app/controllers/doorkeeper/authorizations_controller.rb:108:in `strategy'
doorkeeper (5.6.4) app/controllers/doorkeeper/authorizations_controller.rb:104:in `authorization'
doorkeeper (5.6.4) app/controllers/doorkeeper/authorizations_controller.rb:20:in `destroy'
actionpack (7.0.4.2) lib/action_controller/metal/basic_implicit_render.rb:6:in `send_action'
actionpack (7.0.4.2) lib/abstract_controller/base.rb:215:in `process_action'
actionpack (7.0.4.2) lib/action_controller/metal/rendering.rb:53:in `process_action'
actionpack (7.0.4.2) lib/abstract_controller/callbacks.rb:234:in `block in process_action'
activesupport (7.0.4.2) lib/active_support/callbacks.rb:107:in `run_callbacks'
actionpack (7.0.4.2) lib/abstract_controller/callbacks.rb:233:in `process_action'
actionpack (7.0.4.2) lib/action_controller/metal/rescue.rb:22:in `process_action'
actionpack (7.0.4.2) lib/action_controller/metal/instrumentation.rb:67:in `block in process_action'
activesupport (7.0.4.2) lib/active_support/notifications.rb:206:in `block in instrument'
activesupport (7.0.4.2) lib/active_support/notifications/instrumenter.rb:24:in `instrument'
activesupport (7.0.4.2) lib/active_support/notifications.rb:206:in `instrument'
actionpack (7.0.4.2) lib/action_controller/metal/instrumentation.rb:66:in `process_action'
actionpack (7.0.4.2) lib/action_controller/metal/params_wrapper.rb:259:in `process_action'
activerecord (7.0.4.2) lib/active_record/railties/controller_runtime.rb:27:in `process_action'
actionpack (7.0.4.2) lib/abstract_controller/base.rb:151:in `process'
actionpack (7.0.4.2) lib/action_controller/metal.rb:188:in `dispatch'
actionpack (7.0.4.2) lib/action_controller/metal.rb:251:in `dispatch'
actionpack (7.0.4.2) lib/action_dispatch/routing/route_set.rb:49:in `dispatch'
actionpack (7.0.4.2) lib/action_dispatch/routing/route_set.rb:32:in `serve'
actionpack (7.0.4.2) lib/action_dispatch/journey/router.rb:50:in `block in serve'
actionpack (7.0.4.2) lib/action_dispatch/journey/router.rb:32:in `each'
actionpack (7.0.4.2) lib/action_dispatch/journey/router.rb:32:in `serve'
actionpack (7.0.4.2) lib/action_dispatch/routing/route_set.rb:852:in `call'
rack (2.2.6.2) lib/rack/etag.rb:27:in `call'
rack (2.2.6.2) lib/rack/conditional_get.rb:40:in `call'
rack (2.2.6.2) lib/rack/head.rb:12:in `call'
activerecord (7.0.4.2) lib/active_record/migration.rb:603:in `call'
actionpack (7.0.4.2) lib/action_dispatch/middleware/callbacks.rb:27:in `block in call'
activesupport (7.0.4.2) lib/active_support/callbacks.rb:99:in `run_callbacks'
actionpack (7.0.4.2) lib/action_dispatch/middleware/callbacks.rb:26:in `call'
actionpack (7.0.4.2) lib/action_dispatch/middleware/executor.rb:14:in `call'
actionpack (7.0.4.2) lib/action_dispatch/middleware/actionable_exceptions.rb:17:in `call'
actionpack (7.0.4.2) lib/action_dispatch/middleware/debug_exceptions.rb:28:in `call'
actionpack (7.0.4.2) lib/action_dispatch/middleware/show_exceptions.rb:26:in `call'
railties (7.0.4.2) lib/rails/rack/logger.rb:40:in `call_app'
railties (7.0.4.2) lib/rails/rack/logger.rb:25:in `block in call'
activesupport (7.0.4.2) lib/active_support/tagged_logging.rb:99:in `block in tagged'
activesupport (7.0.4.2) lib/active_support/tagged_logging.rb:37:in `tagged'
activesupport (7.0.4.2) lib/active_support/tagged_logging.rb:99:in `tagged'
railties (7.0.4.2) lib/rails/rack/logger.rb:25:in `call'
actionpack (7.0.4.2) lib/action_dispatch/middleware/remote_ip.rb:93:in `call'
actionpack (7.0.4.2) lib/action_dispatch/middleware/request_id.rb:26:in `call'
rack (2.2.6.2) lib/rack/runtime.rb:22:in `call'
activesupport (7.0.4.2) lib/active_support/cache/strategy/local_cache_middleware.rb:29:in `call'
actionpack (7.0.4.2) lib/action_dispatch/middleware/server_timing.rb:61:in `block in call'
actionpack (7.0.4.2) lib/action_dispatch/middleware/server_timing.rb:26:in `collect_events'
actionpack (7.0.4.2) lib/action_dispatch/middleware/server_timing.rb:60:in `call'
actionpack (7.0.4.2) lib/action_dispatch/middleware/executor.rb:14:in `call'
actionpack (7.0.4.2) lib/action_dispatch/middleware/static.rb:23:in `call'
rack (2.2.6.2) lib/rack/sendfile.rb:110:in `call'
actionpack (7.0.4.2) lib/action_dispatch/middleware/host_authorization.rb:137:in `call'
railties (7.0.4.2) lib/rails/engine.rb:530:in `call'
puma (5.6.5) lib/puma/configuration.rb:252:in `call'
puma (5.6.5) lib/puma/request.rb:77:in `block in handle_request'
puma (5.6.5) lib/puma/thread_pool.rb:340:in `with_force_shutdown'
puma (5.6.5) lib/puma/request.rb:76:in `handle_request'
puma (5.6.5) lib/puma/server.rb:443:in `process_client'
puma (5.6.5) lib/puma/thread_pool.rb:147:in `block in spawn_thread'
```
