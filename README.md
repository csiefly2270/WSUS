# WSUS
@echo off
:input_ip
cls
echo ========================================
echo         清除指定 IP 的 ARP 快取
echo ========================================
echo.
set /p target_ip=請輸入要刪除的 IP 位址 (例如: 192.168.70.56): 

:: 檢查輸入是否為空
if "%target_ip%"=="" (
    echo [錯誤] 未輸入 IP 位址，請重新輸入！
    timeout /t 2 >nul
    goto input_ip
)

echo.
echo ----------------------------------------
echo 正在刪除 ARP 紀錄: %target_ip%
echo ----------------------------------------

:: 執行清除指定 IP 的 ARP 指令
arp -d %target_ip%

if %errorlevel% equ 0 (
    echo [成功] 已送出刪除命令 (%target_ip%)。
) else (
    echo [失敗] 執行失敗，請確認是否已取得管理員權限或 IP 格式是否正確。
)

echo.
echo ----------------------------------------
echo 當前 %target_ip% 的 ARP 狀態：
arp -a | findstr "%target_ip%"
if %errorlevel% neq 0 (
    echo (快取列表中已無 %target_ip% 之紀錄)
)
echo ----------------------------------------
echo.

set /p retry=是否要繼續清除其他 IP？(Y/N): 
if /i "%retry%"=="Y" goto input_ip

echo 感謝使用，按任意鍵結束程式...
pause >nul
