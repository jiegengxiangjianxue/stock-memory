---
name: compose_pull_to_refresh_timing_pitfall
description: Compose Material3 PullToRefreshBox 的 onRefresh 是 suspend，refresh() 内部 launch 会导致无刷新反馈；不应返回 Job.join()
type: fact
---
Compose Material3 `PullToRefreshBox` 的 `onRefresh` 是 suspend 函数。若 `ViewModel.refresh()` 返回 `Unit` 并在内部 `viewModelScope.launch`，调用方拿不到完成时机；依赖 `repo.isLoading` 驱动 `isRefreshing` 时，`isLoading` 可能在回调到达前就变 false，导致下拉后无视觉反馈。错误修法是让 `refresh()` 返回 `Job` 并在 `onRefresh` 中 `join()`——会编译报错（`suspend fun join()` 不能在非协程中调用）。正确做法：让 ViewModel 暴露 `refreshing` 状态，UI 层 collect 后传给 `PullToRefreshBox(isRefreshing = ...)`，`onRefresh` 保持简单调用 `refresh()`。