# optuna
- hyper-parameter tuner

## [[error]]
- `hyperparameter_search` 에러
  - hyperparameter_search 시에는 `model_init` 을 통해서 새로운 모델을 새롭게 생성한다
  - 이 과정에서 tokenizer 가 special token 등을 추가함으로 인해서 vocab size 가 달라지면 아래와 같은 알기 어려운 에러가 발생한다
  - `model_init` 함수에서 아래와 같이 조절이 필요
    ```python 
    def model_init():
        model = T5ForConditionalGeneration.from_pretrained(model_name)
        # 아래 부분
        model.resize_token_embeddings(len(tokenizer))
        return model
    ```
  - 에러 로그
```sh
  0%|          | 0/15580 [00:00<?, ?it/s]/opt/conda/conda-bld/pytorch_1695392020201/work/aten/src/ATen/native/cuda/Indexing.cu:1292: indexSelectLargeIndex: block: [222,0,0], thread: [96,0,0] Assertion `srcIndex < srcSelectDimSize` failed.
/opt/conda/conda-bld/pytorch_1695392020201/work/aten/src/ATen/native/cuda/Indexing.cu:1292: indexSelectLargeIndex: block: [222,0,0], thread: [97,0,0] Assertion `srcIndex < srcSelectDimSize` failed.
/opt/conda/conda-bld/pytorch_1695392020201/work/aten/src/ATen/native/cuda/Indexing.cu:1292: indexSelectLargeIndex: block: [222,0,0], thread: [98,0,0] Assertion `srcIndex < srcSelectDimSize` failed.
/opt/conda/conda-bld/pytorch_1695392020201/work/aten/src/ATen/native/cuda/Indexing.cu:1292: indexSelectLargeIndex: block: [222,0,0], thread: [99,0,0] Assertion `srcIndex < srcSelectDimSize` failed.
/opt/conda/conda-bld/pytorch_1695392020201/work/aten/src/ATen/native/cuda/Indexing.cu:1292: indexSelectLargeIndex: block: [222,0,0], thread: [100,0,0] Assertion `srcIndex < srcSelectDimSize` failed.
/opt/conda/conda-bld/pytorch_1695392020201/work/aten/src/ATen/native/cuda/Indexing.cu:1292: indexSelectLargeIndex: block: [222,0,0], thread: [101,0,0] Assertion `srcIndex < srcSelectDimSize` failed.
/opt/conda/conda-bld/pytorch_1695392020201/work/aten/src/ATen/native/cuda/Indexing.cu:1292: indexSelectLargeIndex: block: [222,0,0], thread: [102,0,0] Assertion `srcIndex < srcSelectDimSize` failed.
/opt/conda/conda-bld/pytorch_1695392020201/work/aten/src/ATen/native/cuda/Indexing.cu:1292: indexSelectLargeIndex: block: [222,0,0], thread: [103,0,0] Assertion `srcIndex < srcSelectDimSize` failed.
/opt/conda/conda-bld/pytorch_1695392020201/work/aten/src/ATen/native/cuda/Indexing.cu:1292: indexSelectLargeIndex: block: [222,0,0], thread: [104,0,0] Assertion `srcIndex < srcSelectDimSize` failed.
/opt/conda/conda-bld/pytorch_1695392020201/work/aten/src/ATen/native/cuda/Indexing.cu:1292: indexSelectLargeIndex: block: [222,0,0], thread: [105,0,0] Assertion `srcIndex < srcSelectDimSize` failed.
/opt/conda/conda-bld/pytorch_1695392020201/work/aten/src/ATen/native/cuda/Indexing.cu:1292: indexSelectLargeIndex: block: [222,0,0], thread: [106,0,0] Assertion `srcIndex < srcSelectDimSize` failed.
/opt/conda/conda-bld/pytorch_1695392020201/work/aten/src/ATen/native/cuda/Indexing.cu:1292: indexSelectLargeIndex: block: [222,0,0], thread: [107,0,0] Assertion `srcIndex < srcSelectDimSize` failed.
/opt/conda/conda-bld/pytorch_1695392020201/work/aten/src/ATen/native/cuda/Indexing.cu:1292: indexSelectLargeIndex: block: [222,0,0], thread: [108,0,0] Assertion `srcIndex < srcSelectDimSize` failed.
/opt/conda/conda-bld/pytorch_1695392020201/work/aten/src/ATen/native/cuda/Indexing.cu:1292: indexSelectLargeIndex: block: [222,0,0], thread: [109,0,0] Assertion `srcIndex < srcSelectDimSize` failed.
/opt/conda/conda-bld/pytorch_1695392020201/work/aten/src/ATen/native/cuda/Indexing.cu:1292: indexSelectLargeIndex: block: [222,0,0], thread: [110,0,0] Assertion `srcIndex < srcSelectDimSize` failed.
/opt/conda/conda-bld/pytorch_1695392020201/work/aten/src/ATen/native/cuda/Indexing.cu:1292: indexSelectLargeIndex: block: [222,0,0], thread: [111,0,0] Assertion `srcIndex < srcSelectDimSize` failed.
/opt/conda/conda-bld/pytorch_1695392020201/work/aten/src/ATen/native/cuda/Indexing.cu:1292: indexSelectLargeIndex: block: [222,0,0], thread: [112,0,0] Assertion `srcIndex < srcSelectDimSize` failed.
/opt/conda/conda-bld/pytorch_1695392020201/work/aten/src/ATen/native/cuda/Indexing.cu:1292: indexSelectLargeIndex: block: [222,0,0], thread: [113,0,0] Assertion `srcIndex < srcSelectDimSize` failed.
/opt/conda/conda-bld/pytorch_1695392020201/work/aten/src/ATen/native/cuda/Indexing.cu:1292: indexSelectLargeIndex: block: [222,0,0], thread: [114,0,0] Assertion `srcIndex < srcSelectDimSize` failed.
/opt/conda/conda-bld/pytorch_1695392020201/work/aten/src/ATen/native/cuda/Indexing.cu:1292: indexSelectLargeIndex: block: [222,0,0], thread: [115,0,0] Assertion `srcIndex < srcSelectDimSize` failed.
/opt/conda/conda-bld/pytorch_1695392020201/work/aten/src/ATen/native/cuda/Indexing.cu:1292: indexSelectLargeIndex: block: [222,0,0], thread: [116,0,0] Assertion `srcIndex < srcSelectDimSize` failed.
/opt/conda/conda-bld/pytorch_1695392020201/work/aten/src/ATen/native/cuda/Indexing.cu:1292: indexSelectLargeIndex: block: [222,0,0], thread: [117,0,0] Assertion `srcIndex < srcSelectDimSize` failed.
/opt/conda/conda-bld/pytorch_1695392020201/work/aten/src/ATen/native/cuda/Indexing.cu:1292: indexSelectLargeIndex: block: [222,0,0], thread: [118,0,0] Assertion `srcIndex < srcSelectDimSize` failed.
/opt/conda/conda-bld/pytorch_1695392020201/work/aten/src/ATen/native/cuda/Indexing.cu:1292: indexSelectLargeIndex: block: [222,0,0], thread: [119,0,0] Assertion `srcIndex < srcSelectDimSize` failed.
/opt/conda/conda-bld/pytorch_1695392020201/work/aten/src/ATen/native/cuda/Indexing.cu:1292: indexSelectLargeIndex: block: [222,0,0], thread: [120,0,0] Assertion `srcIndex < srcSelectDimSize` failed.
/opt/conda/conda-bld/pytorch_1695392020201/work/aten/src/ATen/native/cuda/Indexing.cu:1292: indexSelectLargeIndex: block: [222,0,0], thread: [121,0,0] Assertion `srcIndex < srcSelectDimSize` failed.
/opt/conda/conda-bld/pytorch_1695392020201/work/aten/src/ATen/native/cuda/Indexing.cu:1292: indexSelectLargeIndex: block: [222,0,0], thread: [122,0,0] Assertion `srcIndex < srcSelectDimSize` failed.
/opt/conda/conda-bld/pytorch_1695392020201/work/aten/src/ATen/native/cuda/Indexing.cu:1292: indexSelectLargeIndex: block: [222,0,0], thread: [123,0,0] Assertion `srcIndex < srcSelectDimSize` failed.
/opt/conda/conda-bld/pytorch_1695392020201/work/aten/src/ATen/native/cuda/Indexing.cu:1292: indexSelectLargeIndex: block: [222,0,0], thread: [124,0,0] Assertion `srcIndex < srcSelectDimSize` failed.
/opt/conda/conda-bld/pytorch_1695392020201/work/aten/src/ATen/native/cuda/Indexing.cu:1292: indexSelectLargeIndex: block: [222,0,0], thread: [125,0,0] Assertion `srcIndex < srcSelectDimSize` failed.
/opt/conda/conda-bld/pytorch_1695392020201/work/aten/src/ATen/native/cuda/Indexing.cu:1292: indexSelectLargeIndex: block: [222,0,0], thread: [126,0,0] Assertion `srcIndex < srcSelectDimSize` failed.
/opt/conda/conda-bld/pytorch_1695392020201/work/aten/src/ATen/native/cuda/Indexing.cu:1292: indexSelectLargeIndex: block: [222,0,0], thread: [127,0,0] Assertion `srcIndex < srcSelectDimSize` failed.
/opt/conda/conda-bld/pytorch_1695392020201/work/aten/src/ATen/native/cuda/Indexing.cu:1292: indexSelectLargeIndex: block: [59,0,0], thread: [32,0,0] Assertion `srcIndex < srcSelectDimSize` failed.
/opt/conda/conda-bld/pytorch_1695392020201/work/aten/src/ATen/native/cuda/Indexing.cu:1292: indexSelectLargeIndex: block: [59,0,0], thread: [33,0,0] Assertion `srcIndex < srcSelectDimSize` failed.
/opt/conda/conda-bld/pytorch_1695392020201/work/aten/src/ATen/native/cuda/Indexing.cu:1292: indexSelectLargeIndex: block: [59,0,0], thread: [34,0,0] Assertion `srcIndex < srcSelectDimSize` failed.
/opt/conda/conda-bld/pytorch_1695392020201/work/aten/src/ATen/native/cuda/Indexing.cu:1292: indexSelectLargeIndex: block: [59,0,0], thread: [35,0,0] Assertion `srcIndex < srcSelectDimSize` failed.
/opt/conda/conda-bld/pytorch_1695392020201/work/aten/src/ATen/native/cuda/Indexing.cu:1292: indexSelectLargeIndex: block: [59,0,0], thread: [36,0,0] Assertion `srcIndex < srcSelectDimSize` failed.
/opt/conda/conda-bld/pytorch_1695392020201/work/aten/src/ATen/native/cuda/Indexing.cu:1292: indexSelectLargeIndex: block: [59,0,0], thread: [37,0,0] Assertion `srcIndex < srcSelectDimSize` failed.
/opt/conda/conda-bld/pytorch_1695392020201/work/aten/src/ATen/native/cuda/Indexing.cu:1292: indexSelectLargeIndex: block: [59,0,0], thread: [38,0,0] Assertion `srcIndex < srcSelectDimSize` failed.
/opt/conda/conda-bld/pytorch_1695392020201/work/aten/src/ATen/native/cuda/Indexing.cu:1292: indexSelectLargeIndex: block: [59,0,0], thread: [39,0,0] Assertion `srcIndex < srcSelectDimSize` failed.
/opt/conda/conda-bld/pytorch_1695392020201/work/aten/src/ATen/native/cuda/Indexing.cu:1292: indexSelectLargeIndex: block: [59,0,0], thread: [40,0,0] Assertion `srcIndex < srcSelectDimSize` failed.
/opt/conda/conda-bld/pytorch_1695392020201/work/aten/src/ATen/native/cuda/Indexing.cu:1292: indexSelectLargeIndex: block: [59,0,0], thread: [41,0,0] Assertion `srcIndex < srcSelectDimSize` failed.
/opt/conda/conda-bld/pytorch_1695392020201/work/aten/src/ATen/native/cuda/Indexing.cu:1292: indexSelectLargeIndex: block: [59,0,0], thread: [42,0,0] Assertion `srcIndex < srcSelectDimSize` failed.
/opt/conda/conda-bld/pytorch_1695392020201/work/aten/src/ATen/native/cuda/Indexing.cu:1292: indexSelectLargeIndex: block: [59,0,0], thread: [43,0,0] Assertion `srcIndex < srcSelectDimSize` failed.
/opt/conda/conda-bld/pytorch_1695392020201/work/aten/src/ATen/native/cuda/Indexing.cu:1292: indexSelectLargeIndex: block: [59,0,0], thread: [44,0,0] Assertion `srcIndex < srcSelectDimSize` failed.
/opt/conda/conda-bld/pytorch_1695392020201/work/aten/src/ATen/native/cuda/Indexing.cu:1292: indexSelectLargeIndex: block: [59,0,0], thread: [45,0,0] Assertion `srcIndex < srcSelectDimSize` failed.
/opt/conda/conda-bld/pytorch_1695392020201/work/aten/src/ATen/native/cuda/Indexing.cu:1292: indexSelectLargeIndex: block: [59,0,0], thread: [46,0,0] Assertion `srcIndex < srcSelectDimSize` failed.
/opt/conda/conda-bld/pytorch_1695392020201/work/aten/src/ATen/native/cuda/Indexing.cu:1292: indexSelectLargeIndex: block: [59,0,0], thread: [47,0,0] Assertion `srcIndex < srcSelectDimSize` failed.
/opt/conda/conda-bld/pytorch_1695392020201/work/aten/src/ATen/native/cuda/Indexing.cu:1292: indexSelectLargeIndex: block: [59,0,0], thread: [48,0,0] Assertion `srcIndex < srcSelectDimSize` failed.
/opt/conda/conda-bld/pytorch_1695392020201/work/aten/src/ATen/native/cuda/Indexing.cu:1292: indexSelectLargeIndex: block: [59,0,0], thread: [49,0,0] Assertion `srcIndex < srcSelectDimSize` failed.
/opt/conda/conda-bld/pytorch_1695392020201/work/aten/src/ATen/native/cuda/Indexing.cu:1292: indexSelectLargeIndex: block: [59,0,0], thread: [50,0,0] Assertion `srcIndex < srcSelectDimSize` failed.
/opt/conda/conda-bld/pytorch_1695392020201/work/aten/src/ATen/native/cuda/Indexing.cu:1292: indexSelectLargeIndex: block: [59,0,0], thread: [51,0,0] Assertion `srcIndex < srcSelectDimSize` failed.
/opt/conda/conda-bld/pytorch_1695392020201/work/aten/src/ATen/native/cuda/Indexing.cu:1292: indexSelectLargeIndex: block: [59,0,0], thread: [52,0,0] Assertion `srcIndex < srcSelectDimSize` failed.
/opt/conda/conda-bld/pytorch_1695392020201/work/aten/src/ATen/native/cuda/Indexing.cu:1292: indexSelectLargeIndex: block: [59,0,0], thread: [53,0,0] Assertion `srcIndex < srcSelectDimSize` failed.
/opt/conda/conda-bld/pytorch_1695392020201/work/aten/src/ATen/native/cuda/Indexing.cu:1292: indexSelectLargeIndex: block: [59,0,0], thread: [54,0,0] Assertion `srcIndex < srcSelectDimSize` failed.
/opt/conda/conda-bld/pytorch_1695392020201/work/aten/src/ATen/native/cuda/Indexing.cu:1292: indexSelectLargeIndex: block: [59,0,0], thread: [55,0,0] Assertion `srcIndex < srcSelectDimSize` failed.
/opt/conda/conda-bld/pytorch_1695392020201/work/aten/src/ATen/native/cuda/Indexing.cu:1292: indexSelectLargeIndex: block: [59,0,0], thread: [56,0,0] Assertion `srcIndex < srcSelectDimSize` failed.
/opt/conda/conda-bld/pytorch_1695392020201/work/aten/src/ATen/native/cuda/Indexing.cu:1292: indexSelectLargeIndex: block: [59,0,0], thread: [57,0,0] Assertion `srcIndex < srcSelectDimSize` failed.
/opt/conda/conda-bld/pytorch_1695392020201/work/aten/src/ATen/native/cuda/Indexing.cu:1292: indexSelectLargeIndex: block: [59,0,0], thread: [58,0,0] Assertion `srcIndex < srcSelectDimSize` failed.
/opt/conda/conda-bld/pytorch_1695392020201/work/aten/src/ATen/native/cuda/Indexing.cu:1292: indexSelectLargeIndex: block: [59,0,0], thread: [59,0,0] Assertion `srcIndex < srcSelectDimSize` failed.
/opt/conda/conda-bld/pytorch_1695392020201/work/aten/src/ATen/native/cuda/Indexing.cu:1292: indexSelectLargeIndex: block: [59,0,0], thread: [60,0,0] Assertion `srcIndex < srcSelectDimSize` failed.
/opt/conda/conda-bld/pytorch_1695392020201/work/aten/src/ATen/native/cuda/Indexing.cu:1292: indexSelectLargeIndex: block: [59,0,0], thread: [61,0,0] Assertion `srcIndex < srcSelectDimSize` failed.
/opt/conda/conda-bld/pytorch_1695392020201/work/aten/src/ATen/native/cuda/Indexing.cu:1292: indexSelectLargeIndex: block: [59,0,0], thread: [62,0,0] Assertion `srcIndex < srcSelectDimSize` failed.
/opt/conda/conda-bld/pytorch_1695392020201/work/aten/src/ATen/native/cuda/Indexing.cu:1292: indexSelectLargeIndex: block: [59,0,0], thread: [63,0,0] Assertion `srcIndex < srcSelectDimSize` failed.
[W 2024-11-28 16:25:27,015] Trial 0 failed with parameters: {'learning_rate': 0.0005876652453419318} because of the following error: RuntimeError('CUDA error: device-side assert triggered\nCUDA kernel errors might be asynchronously reported at some other API call, so the stacktrace below might be incorrect.\nFor debugging consider passing CUDA_LAUNCH_BLOCKING=1.\nCompile with `TORCH_USE_CUDA_DSA` to enable device-side assertions.\n').
Traceback (most recent call last):
  File "/opt/conda/lib/python3.10/site-packages/optuna/study/_optimize.py", line 197, in _run_trial
    value_or_values = func(trial)
  File "/opt/conda/lib/python3.10/site-packages/transformers/integrations/integration_utils.py", line 248, in _objective
    trainer.train(resume_from_checkpoint=checkpoint, trial=trial)
  File "/opt/conda/lib/python3.10/site-packages/mlflow/utils/autologging_utils/safety.py", line 593, in safe_patch_function
    patch_function(call_original, *args, **kwargs)
  File "/opt/conda/lib/python3.10/site-packages/mlflow/transformers/__init__.py", line 2931, in train
    return original(*args, **kwargs)
  File "/opt/conda/lib/python3.10/site-packages/mlflow/utils/autologging_utils/safety.py", line 574, in call_original
    return call_original_fn_with_event_logging(_original_fn, og_args, og_kwargs)
  File "/opt/conda/lib/python3.10/site-packages/mlflow/utils/autologging_utils/safety.py", line 509, in call_original_fn_with_event_logging
    original_fn_result = original_fn(*og_args, **og_kwargs)
  File "/opt/conda/lib/python3.10/site-packages/mlflow/utils/autologging_utils/safety.py", line 571, in _original_fn
    original_result = original(*_og_args, **_og_kwargs)
  File "/opt/conda/lib/python3.10/site-packages/transformers/trainer.py", line 2123, in train
    return inner_training_loop(
  File "/opt/conda/lib/python3.10/site-packages/transformers/trainer.py", line 2481, in _inner_training_loop
    tr_loss_step = self.training_step(model, inputs, num_items_in_batch)
  File "/opt/conda/lib/python3.10/site-packages/transformers/trainer.py", line 3579, in training_step
    loss = self.compute_loss(model, inputs, num_items_in_batch=num_items_in_batch)
  File "/opt/conda/lib/python3.10/site-packages/transformers/trainer.py", line 3633, in compute_loss
    outputs = model(**inputs)
  File "/opt/conda/lib/python3.10/site-packages/torch/nn/modules/module.py", line 1518, in _wrapped_call_impl
    return self._call_impl(*args, **kwargs)
  File "/opt/conda/lib/python3.10/site-packages/torch/nn/modules/module.py", line 1527, in _call_impl
    return forward_call(*args, **kwargs)
  File "/opt/conda/lib/python3.10/site-packages/accelerate/utils/operations.py", line 823, in forward
    return model_forward(*args, **kwargs)
  File "/opt/conda/lib/python3.10/site-packages/accelerate/utils/operations.py", line 811, in __call__
    return convert_to_fp32(self.model_forward(*args, **kwargs))
  File "/opt/conda/lib/python3.10/site-packages/torch/amp/autocast_mode.py", line 16, in decorate_autocast
    return func(*args, **kwargs)
  File "/opt/conda/lib/python3.10/site-packages/transformers/models/t5/modeling_t5.py", line 1854, in forward
    encoder_outputs = self.encoder(
  File "/opt/conda/lib/python3.10/site-packages/torch/nn/modules/module.py", line 1518, in _wrapped_call_impl
    return self._call_impl(*args, **kwargs)
  File "/opt/conda/lib/python3.10/site-packages/torch/nn/modules/module.py", line 1527, in _call_impl
    return forward_call(*args, **kwargs)
  File "/opt/conda/lib/python3.10/site-packages/transformers/models/t5/modeling_t5.py", line 1034, in forward
    cache_position = torch.arange(
RuntimeError: CUDA error: device-side assert triggered
CUDA kernel errors might be asynchronously reported at some other API call, so the stacktrace below might be incorrect.
For debugging consider passing CUDA_LAUNCH_BLOCKING=1.
Compile with `TORCH_USE_CUDA_DSA` to enable device-side assertions.

[W 2024-11-28 16:25:27,017] Trial 0 failed with value None.
Traceback (most recent call last):
  File "/data/ephemeral/home/src/model_t5/model.py", line 270, in <module>
    best_params = trainer.hyperparameter_search(
  File "/opt/conda/lib/python3.10/site-packages/transformers/trainer.py", line 3473, in hyperparameter_search
    best_run = backend_obj.run(self, n_trials, direction, **kwargs)
  File "/opt/conda/lib/python3.10/site-packages/transformers/hyperparameter_search.py", line 72, in run
    return run_hp_search_optuna(trainer, n_trials, direction, **kwargs)
  File "/opt/conda/lib/python3.10/site-packages/transformers/integrations/integration_utils.py", line 261, in run_hp_search_optuna
    study.optimize(_objective, n_trials=n_trials, timeout=timeout, n_jobs=n_jobs, gc_after_trial=gc_after_trial)
  File "/opt/conda/lib/python3.10/site-packages/optuna/study/study.py", line 475, in optimize
    _optimize(
  File "/opt/conda/lib/python3.10/site-packages/optuna/study/_optimize.py", line 63, in _optimize
    _optimize_sequential(
  File "/opt/conda/lib/python3.10/site-packages/optuna/study/_optimize.py", line 160, in _optimize_sequential
    frozen_trial = _run_trial(study, func, catch)
  File "/opt/conda/lib/python3.10/site-packages/optuna/study/_optimize.py", line 248, in _run_trial
    raise func_err
  File "/opt/conda/lib/python3.10/site-packages/optuna/study/_optimize.py", line 197, in _run_trial
    value_or_values = func(trial)
  File "/opt/conda/lib/python3.10/site-packages/transformers/integrations/integration_utils.py", line 248, in _objective
    trainer.train(resume_from_checkpoint=checkpoint, trial=trial)
  File "/opt/conda/lib/python3.10/site-packages/mlflow/utils/autologging_utils/safety.py", line 593, in safe_patch_function
    patch_function(call_original, *args, **kwargs)
  File "/opt/conda/lib/python3.10/site-packages/mlflow/transformers/__init__.py", line 2931, in train
    return original(*args, **kwargs)
  File "/opt/conda/lib/python3.10/site-packages/mlflow/utils/autologging_utils/safety.py", line 574, in call_original
    return call_original_fn_with_event_logging(_original_fn, og_args, og_kwargs)
  File "/opt/conda/lib/python3.10/site-packages/mlflow/utils/autologging_utils/safety.py", line 509, in call_original_fn_with_event_logging
    original_fn_result = original_fn(*og_args, **og_kwargs)
  File "/opt/conda/lib/python3.10/site-packages/mlflow/utils/autologging_utils/safety.py", line 571, in _original_fn
    original_result = original(*_og_args, **_og_kwargs)
  File "/opt/conda/lib/python3.10/site-packages/transformers/trainer.py", line 2123, in train
    return inner_training_loop(
  File "/opt/conda/lib/python3.10/site-packages/transformers/trainer.py", line 2481, in _inner_training_loop
    tr_loss_step = self.training_step(model, inputs, num_items_in_batch)
  File "/opt/conda/lib/python3.10/site-packages/transformers/trainer.py", line 3579, in training_step
    loss = self.compute_loss(model, inputs, num_items_in_batch=num_items_in_batch)
  File "/opt/conda/lib/python3.10/site-packages/transformers/trainer.py", line 3633, in compute_loss
    outputs = model(**inputs)
  File "/opt/conda/lib/python3.10/site-packages/torch/nn/modules/module.py", line 1518, in _wrapped_call_impl
    return self._call_impl(*args, **kwargs)
  File "/opt/conda/lib/python3.10/site-packages/torch/nn/modules/module.py", line 1527, in _call_impl
    return forward_call(*args, **kwargs)
  File "/opt/conda/lib/python3.10/site-packages/accelerate/utils/operations.py", line 823, in forward
    return model_forward(*args, **kwargs)
  File "/opt/conda/lib/python3.10/site-packages/accelerate/utils/operations.py", line 811, in __call__
    return convert_to_fp32(self.model_forward(*args, **kwargs))
  File "/opt/conda/lib/python3.10/site-packages/torch/amp/autocast_mode.py", line 16, in decorate_autocast
    return func(*args, **kwargs)
  File "/opt/conda/lib/python3.10/site-packages/transformers/models/t5/modeling_t5.py", line 1854, in forward
    encoder_outputs = self.encoder(
  File "/opt/conda/lib/python3.10/site-packages/torch/nn/modules/module.py", line 1518, in _wrapped_call_impl
    return self._call_impl(*args, **kwargs)
  File "/opt/conda/lib/python3.10/site-packages/torch/nn/modules/module.py", line 1527, in _call_impl
    return forward_call(*args, **kwargs)
  File "/opt/conda/lib/python3.10/site-packages/transformers/models/t5/modeling_t5.py", line 1034, in forward
    cache_position = torch.arange(
RuntimeError: CUDA error: device-side assert triggered
CUDA kernel errors might be asynchronously reported at some other API call, so the stacktrace below might be incorrect.
For debugging consider passing CUDA_LAUNCH_BLOCKING=1.
Compile with `TORCH_USE_CUDA_DSA` to enable device-side assertions.
```

## link
- [[mlflow]]
- [[python]]
