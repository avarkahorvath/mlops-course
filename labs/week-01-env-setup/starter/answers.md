## Task 1
In this example, the random seed controls randomness in the model and also the selection of rows end up in the training (and testing) data. It is good practice to check for multiple random seeds, as it reveals model stability. For optimalization, it does not make sense.

## Task 4
Error shows up in `src/week_01_env_setup/config.py (:41: ValueError)`, `test_load_settings_returns_valid_settings` and `test_split_ratios` fails. Catching errors early is better, as it gives a clear message about what went wrong, and does not waste resources.
