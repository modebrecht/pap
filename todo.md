[x] #task1 Fix stale `state.pendingLinkFrom` when a source node gets deleted while link mode is active. Deleting the node should cancel link mode to avoid orphan connectors and misleading toasts.
[] #task2 Clear the JSON import file input after processing so the same file can be re-imported consecutively.
[] #task3 Revoke the object URL used during JSON export after triggering the download to prevent leaks.
