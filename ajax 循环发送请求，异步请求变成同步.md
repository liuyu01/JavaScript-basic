```
ajax 循环发送请求，异步请求变成同步
batchApprovalIndex = 0;
function batchApproval() {
    var rows = GetSelectRows();  
    if (rows.length == 0) {
        topWin.ShowMsg('info', '至少选择一条数据！');
        return;
    }
    var gridId = null;
    if (batchApprovalIndex >= rows.length) {
        RefreshGrid(gridId); //刷新网格
        batchApprovalIndex = 0;
        topWin.ShowMsg('success', '操作成功！');
        return;
    }
    var url = '/Bpm/ApprovalProcess.html';
    const item = rows[batchApprovalIndex];
    let taskId = item.Id;
    var params = {
        toDoTaskId: taskId,
        approvalOpinions: "批量审批",
        workAction: 3
    }
    var info = "当前第【" + (batchApprovalIndex + 1) + "】个,共" + rows.length + "个";
    $.ajax({
        async: true,
        type: 'post',
        url: url,
        data: params,
        dataType: "json",
        beforeSend: function () {
            console.log(new Date());
            topWin.OpenWaitDialog(info);
        },
        success: function (result) {
            topWin.CloseWaitDialog();
            if (result.Success) {
                //topWin.ShowMsg('success', '操作成功！');
                batchApprovalIndex = batchApprovalIndex + 1;
                //手动修改毫秒数
                setTimeout(batchApproval,1000)
            } else {
                topWin.ShowMsg("info", result.Message, null, null, 5);
                RefreshGrid(gridId); //刷新网格
                batchApprovalIndex = 0;
            }
        },
        error: function (err) {
            topWin.CloseWaitDialog();
            topWin.ShowMsg("error", "操作失败，服务端异常！");
            RefreshGrid(gridId); //刷新网格
            batchApprovalIndex = 0;
        }
    });
}
```

