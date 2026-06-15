# Tammy-todo
会议纪要待办管理系统
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=yes">
    <title>会议待办智能解析器 | 图片识别待办事项</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            background: linear-gradient(145deg, #f0f4fa 0%, #e2e8f0 100%);
            font-family: 'Segoe UI', 'Roboto', 'Noto Sans', system-ui, -apple-system, BlinkMacSystemFont, 'Helvetica Neue', sans-serif;
            padding: 2rem 1.5rem;
            min-height: 100vh;
        }

        /* 主容器 */
        .app-container {
            max-width: 1400px;
            margin: 0 auto;
        }

        /* 头部 */
        .hero {
            text-align: center;
            margin-bottom: 2.5rem;
        }

        .hero h1 {
            font-size: 2.4rem;
            font-weight: 700;
            background: linear-gradient(135deg, #1E2A5E, #2C3E6D);
            background-clip: text;
            -webkit-background-clip: text;
            color: transparent;
            letter-spacing: -0.3px;
        }

        .hero p {
            color: #4a5568;
            margin-top: 0.5rem;
            font-size: 1rem;
        }

        /* 双栏网格 */
        .dashboard {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 1.8rem;
        }

        /* 通用卡片 */
        .card {
            background: rgba(255, 255, 255, 0.92);
            backdrop-filter: blur(2px);
            border-radius: 2rem;
            box-shadow: 0 20px 35px -12px rgba(0, 0, 0, 0.15);
            overflow: hidden;
            transition: transform 0.2s, box-shadow 0.2s;
            border: 1px solid rgba(255, 255, 255, 0.5);
        }

        .card-header {
            padding: 1.2rem 1.5rem;
            background: #ffffffcc;
            border-bottom: 2px solid #eef2f6;
            display: flex;
            align-items: center;
            justify-content: space-between;
            flex-wrap: wrap;
            gap: 12px;
        }

        .card-header h2 {
            font-size: 1.4rem;
            font-weight: 600;
            color: #1e293b;
            display: flex;
            align-items: center;
            gap: 8px;
        }

        .badge-icon {
            background: #EFF6FF;
            padding: 6px 12px;
            border-radius: 40px;
            font-size: 0.75rem;
            font-weight: 500;
            color: #2563eb;
        }

        /* 图片上传区域 */
        .image-paste-area {
            padding: 1.5rem;
            border-bottom: 1px solid #eef2f6;
            background: #fafcff;
        }

        .paste-zone {
            background: #ffffff;
            border: 2px dashed #cbd5e1;
            border-radius: 1.2rem;
            padding: 1.8rem 1rem;
            text-align: center;
            cursor: pointer;
            transition: 0.2s;
        }

        .paste-zone:hover {
            border-color: #3b82f6;
            background: #f8fafd;
        }

        .paste-icon {
            font-size: 2.5rem;
            margin-bottom: 10px;
        }

        .paste-zone p {
            color: #334155;
            font-weight: 500;
        }

        .paste-zone small {
            color: #6c757d;
            font-size: 0.75rem;
        }

        .preview-img {
            margin-top: 1rem;
            max-width: 100%;
            border-radius: 1rem;
            border: 1px solid #e2e8f0;
            background: #f1f5f9;
            display: none;
            max-height: 180px;
            object-fit: contain;
        }

        .control-buttons {
            padding: 1rem 1.5rem;
            display: flex;
            gap: 12px;
            justify-content: flex-end;
            background: #f9fbfe;
            border-top: 1px solid #eef2f6;
        }

        /* 按钮设计 */
        .btn {
            border: none;
            padding: 0.55rem 1.2rem;
            border-radius: 2rem;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.2s ease;
            font-size: 0.85rem;
            background: white;
            box-shadow: 0 1px 2px rgba(0,0,0,0.05);
        }

        .btn-primary {
            background: #1e3a8a;
            color: white;
            box-shadow: 0 4px 8px rgba(30, 58, 138, 0.2);
        }

        .btn-primary:hover {
            background: #2563eb;
            transform: translateY(-1px);
        }

        .btn-outline {
            border: 1px solid #cbd5e1;
            background: white;
        }

        .btn-outline:hover {
            background: #f1f5f9;
        }

        /* 文本区 */
        .text-input-area {
            padding: 1.2rem 1.5rem;
        }

        .text-input-area textarea {
            width: 100%;
            border-radius: 1.2rem;
            border: 1px solid #cbd5e6;
            padding: 0.9rem 1rem;
            font-family: 'SF Mono', 'Fira Code', monospace;
            font-size: 0.9rem;
            background: #ffffff;
            resize: vertical;
            transition: 0.2s;
        }

        .text-input-area textarea:focus {
            outline: none;
            border-color: #3b82f6;
            box-shadow: 0 0 0 3px rgba(59,130,246,0.1);
        }

        /* 待办列表区域 */
        .todo-list-container {
            padding: 0 1.2rem 1.2rem 1.2rem;
        }

        .todo-stats {
            display: flex;
            justify-content: space-between;
            align-items: baseline;
            padding: 0.5rem 0 1rem 0;
            font-size: 0.8rem;
            color: #475569;
            border-bottom: 1px dashed #e2e8f0;
            margin-bottom: 1rem;
        }

        .todo-items {
            max-height: 460px;
            overflow-y: auto;
            padding-right: 4px;
        }

        .todo-item {
            display: flex;
            align-items: center;
            justify-content: space-between;
            background: #ffffff;
            border-radius: 1rem;
            padding: 0.75rem 1rem;
            margin-bottom: 0.7rem;
            box-shadow: 0 1px 3px rgba(0,0,0,0.05);
            border: 1px solid #edf2f7;
            transition: all 0.1s;
        }

        .todo-info {
            display: flex;
            flex-direction: column;
            gap: 5px;
            flex: 1;
        }

        .todo-task {
            font-weight: 600;
            color: #0f172a;
            word-break: break-word;
        }

        .todo-meta {
            display: flex;
            gap: 1rem;
            font-size: 0.7rem;
            color: #5b6e8c;
        }

        .todo-meta span {
            background: #f1f5f9;
            padding: 2px 8px;
            border-radius: 20px;
        }

        .delete-btn {
            background: none;
            border: none;
            font-size: 1.2rem;
            cursor: pointer;
            color: #94a3b8;
            padding: 6px;
            border-radius: 40px;
            transition: 0.1s;
        }

        .delete-btn:hover {
            color: #ef4444;
            background: #fee2e2;
        }

        .empty-todo {
            text-align: center;
            padding: 2rem;
            color: #94a3b8;
            font-style: italic;
        }

        /* 右栏滚动 */
        .right-scroll {
            max-height: 85vh;
            overflow-y: auto;
        }

        /* 响应式 */
        @media (max-width: 800px) {
            .dashboard {
                grid-template-columns: 1fr;
                gap: 1.5rem;
            }
            body {
                padding: 1rem;
            }
        }

        /* 解析反馈 */
        .toast-msg {
            position: fixed;
            bottom: 20px;
            left: 50%;
            transform: translateX(-50%);
            background: #1e293b;
            color: white;
            padding: 8px 20px;
            border-radius: 40px;
            font-size: 0.85rem;
            z-index: 1000;
            opacity: 0;
            transition: 0.2s;
            pointer-events: none;
        }

        .loader {
            display: inline-block;
            width: 14px;
            height: 14px;
            border: 2px solid rgba(255,255,255,0.3);
            border-radius: 50%;
            border-top-color: white;
            animation: spin 0.8s linear infinite;
            margin-left: 8px;
        }

        @keyframes spin {
            to { transform: rotate(360deg); }
        }
    </style>
</head>
<body>
<div class="app-container">
    <div class="hero">
        <h1>📋 会议待办 · 智能解析器</h1>
        <p>📸 支持图片粘贴 / 截图一键识别 | 自动提取负责人 & 截止日期</p>
    </div>
    <div class="dashboard">
        <!-- 左侧：图片输入 + 手动文本区 -->
        <div class="card">
            <div class="card-header">
                <h2>📎 导入会议纪要</h2>
                <div class="badge-icon">📸 粘贴图片 或 键入文本</div>
            </div>
            <div class="image-paste-area">
                <div class="paste-zone" id="pasteZone">
                    <div class="paste-icon">🖼️ 📋</div>
                    <p>点击此处 或 按 Ctrl+V 粘贴图片</p>
                    <small>支持 PNG / JPG / WebP / 截图直接粘贴</small>
                </div>
                <img id="previewImage" class="preview-img" alt="预览图片">
                <div style="font-size: 0.7rem; margin-top: 8px; color: #2c3e66; text-align: center;" id="ocrHint">
                    💡 粘贴图片后将自动提取文字并解析待办
                </div>
            </div>
            <div class="text-input-area">
                <label style="font-weight:500; margin-bottom:8px; display:block;">✍️ 或手动编辑/修正文本</label>
                <textarea id="rawTextArea" rows="5" placeholder="示例:
笼筐清洗--GJL，7.1完成
外包装袋油污优化--SK 7.3完成
节拍无压发的产品优化---TM 7.8完成
或者任意格式: 任务名称 @负责人 6.20截止"></textarea>
            </div>
            <div class="control-buttons">
                <button class="btn btn-outline" id="clearAllBtn">🗑️ 清空待办</button>
                <button class="btn btn-primary" id="parseTextBtn">✨ 智能解析文本</button>
            </div>
        </div>

        <!-- 右侧：待办清单 -->
        <div class="card right-scroll">
            <div class="card-header">
                <h2>✅ 智能待办清单</h2>
                <div class="badge-icon" id="todoCountBadge">0项待办</div>
            </div>
            <div class="todo-list-container">
                <div class="todo-stats">
                    <span>📌 从会议纪要提取任务</span>
                    <span>⚡ 点击“✓”可删除</span>
                </div>
                <div id="todoListRoot" class="todo-items">
                    <div class="empty-todo">暂无待办，请粘贴图片或输入文本解析 →</div>
                </div>
            </div>
        </div>
    </div>
</div>
<div id="toastMsg" class="toast-msg"></div>

<script>
    // ---------- 存储待办数据 ----------
    let todoItems = [];   // 每个元素: { id, task, owner, deadline, raw }

    // DOM 元素
    const todoRoot = document.getElementById('todoListRoot');
    const rawTextArea = document.getElementById('rawTextArea');
    const parseTextBtn = document.getElementById('parseTextBtn');
    const clearAllBtn = document.getElementById('clearAllBtn');
    const pasteZone = document.getElementById('pasteZone');
    const previewImg = document.getElementById('previewImage');
    const todoCountBadge = document.getElementById('todoCountBadge');
    const toastMsgDiv = document.getElementById('toastMsg');

    // 显示提示
    function showToast(message, duration = 2000) {
        toastMsgDiv.innerText = message;
        toastMsgDiv.style.opacity = '1';
        setTimeout(() => {
            toastMsgDiv.style.opacity = '0';
        }, duration);
    }

    // 渲染待办列表
    function renderTodos() {
        if (!todoRoot) return;
        if (todoItems.length === 0) {
            todoRoot.innerHTML = `<div class="empty-todo">✨ 暂无待办，请粘贴图片或输入文本解析 →</div>`;
            todoCountBadge.innerText = `0项待办`;
            return;
        }
        todoCountBadge.innerText = `${todoItems.length}项待办`;
        const fragment = document.createDocumentFragment();
        todoItems.forEach(item => {
            const div = document.createElement('div');
            div.className = 'todo-item';
            div.setAttribute('data-id', item.id);
            // 组装内容
            const taskDisplay = item.task || '未命名任务';
            const ownerText = item.owner ? `👤 ${item.owner}` : '';
            const deadlineText = item.deadline ? `📅 ${item.deadline}` : '';
            div.innerHTML = `
                <div class="todo-info">
                    <div class="todo-task">📌 ${escapeHtml(taskDisplay)}</div>
                    <div class="todo-meta">
                        ${ownerText ? `<span>${ownerText}</span>` : ''}
                        ${deadlineText ? `<span>${deadlineText}</span>` : ''}
                    </div>
                </div>
                <button class="delete-btn" data-id="${item.id}" title="删除待办">🗑️</button>
            `;
            fragment.appendChild(div);
        });
        todoRoot.innerHTML = '';
        todoRoot.appendChild(fragment);

        // 绑定删除事件
        document.querySelectorAll('.delete-btn').forEach(btn => {
            btn.addEventListener('click', (e) => {
                e.stopPropagation();
                const id = parseInt(btn.getAttribute('data-id'));
                todoItems = todoItems.filter(t => t.id !== id);
                renderTodos();
                showToast('已移除待办', 1200);
                // 可选同步保存到 localStorage
                saveTodosToLocal();
            });
        });
    }

    // 辅助：转义HTML
    function escapeHtml(str) {
        if (!str) return '';
        return str.replace(/[&<>]/g, function(m) {
            if (m === '&') return '&amp;';
            if (m === '<') return '&lt;';
            if (m === '>') return '&gt;';
            return m;
        }).replace(/[\uD800-\uDBFF][\uDC00-\uDFFF]/g, function(c) {
            return c;
        });
    }

    // 核心解析函数: 输入文本，返回解析出的待办数组 (结构化)
    function parseMeetingNotesToTodos(text) {
        if (!text || typeof text !== 'string') return [];
        const lines = text.split(/\r?\n/);
        const results = [];

        // 增强正则匹配: 常见格式 "任务名--负责人 日期", "任务名--负责人，日期", "任务名---负责人 7.1完成", "任务名 @张三 6.20"
        // 匹配负责人 (支持中英文名 / 缩写), 以及日期模式 (月.日, 月/日, 或 xx月xx日, 以及完成后缀)
        for (let line of lines) {
            line = line.trim();
            if (line.length === 0) continue;
            // 尝试多种模式提取
            let task = '', owner = '', deadline = '';
            
            // 模式1: 典型 "--" 或 "---" 或 "——" 分隔  例: 笼筐清洗--GJL，7.1完成
            let pattern1 = /^(.+?)[\-\–\—]{1,3}\s*([A-Za-z0-9\u4e00-\u9fa5]+)[\s,，]*([\d\.\/月日]+)?(?:完成|截止|due)?/i;
            let match = line.match(pattern1);
            if (match) {
                task = match[1].trim();
                owner = match[2].trim();
                let dateRaw = match[3] ? match[3].trim() : '';
                deadline = normalizeDate(dateRaw);
                if (task && owner) {
                    results.push({ task, owner, deadline: deadline || '' });
                    continue;
                }
            }
            
            // 模式2: 使用 @ 负责人  例: 外包装袋油污优化 @SK 7.3完成
            let atPattern = /^(.+?)\s*[@＠]\s*([A-Za-z0-9\u4e00-\u9fa5]+)[\s,，]*([\d\.\/月日]+)?(?:完成|截止)?/i;
            let atMatch = line.match(atPattern);
            if (atMatch) {
                task = atMatch[1].trim();
                owner = atMatch[2].trim();
                let dateRaw = atMatch[3] ? atMatch[3].trim() : '';
                deadline = normalizeDate(dateRaw);
                if (task && owner) {
                    results.push({ task, owner, deadline });
                    continue;
                }
            }
            
            // 模式3: 更灵活: 负责人+日期 (例: GJL:笼筐清洗 7.1； SK 完成外包装 7.3)
            let flexPattern = /(?:^|[,，])\s*([A-Za-z0-9\u4e00-\u9fa5]+)[\s:：]+(.+?)[\s,，]*([\d\.\/]+[月日]?)?/;
            let flexMatch = line.match(flexPattern);
            if (flexMatch && flexMatch[1] && flexMatch[2]) {
                owner = flexMatch[1].trim();
                task = flexMatch[2].trim();
                let datePart = flexMatch[3] ? flexMatch[3].trim() : '';
                deadline = normalizeDate(datePart);
                if (task && owner) {
                    results.push({ task, owner, deadline });
                    continue;
                }
            }
            
            // 模式4: 兜底: 识别类似 '张三 7.2' 或者包含日期和任务描述，尽力提取负责人 (如果有中文/英文组合)
            let fallbackOwnerRegex = /([A-Z]{2,5}|[A-Z][a-z]+|[a-z]{2,}|[\u4e00-\u9fa5]{2,4})(?=[\s,，]*[\d\.\/月]|完成|截止)/;
            let fallbackDateRegex = /(\d{1,2}[\.\/]\d{1,2})(?:完成|截止)?|(\d{1,2}月\d{1,2}日?)/;
            let ownerFall = line.match(fallbackOwnerRegex);
            let dateFall = line.match(fallbackDateRegex);
            let extractedDate = '';
            if (dateFall) {
                extractedDate = normalizeDate(dateFall[0]);
            }
            if (ownerFall) {
                let possibleOwner = ownerFall[1].trim();
                // 去除开头数字标点
                let cleanedTask = line.replace(ownerFall[0], '').replace(/[\d\.\/月日完截止]/g,'').trim();
                if (cleanedTask.length < 2) cleanedTask = line.substring(0, 30);
                results.push({
                    task: cleanedTask.substring(0, 60),
                    owner: possibleOwner,
                    deadline: extractedDate
                });
                continue;
            }
            
            // 最后: 如果没有任何匹配, 将整行作为任务，无负责人和日期
            if (line.length > 0) {
                results.push({
                    task: line.substring(0, 80),
                    owner: '',
                    deadline: ''
                });
            }
        }
        
        // 去重简单策略: 相同任务文本+负责人合并为一个 (可选)
        const unique = [];
        const mapKey = new Set();
        for (let item of results) {
            const key = `${item.task}|${item.owner}`;
            if (!mapKey.has(key)) {
                mapKey.add(key);
                unique.push(item);
            }
        }
        return unique;
    }
    
    // 标准化日期格式: 输入 "7.1", "7.1完成", "7/1", "7月1日" => 输出 "07月01日" 或保留友好 "7月1日"? 统一为月日展示: M月D日
    function normalizeDate(dateStr) {
        if (!dateStr) return '';
        let cleaned = dateStr.replace(/完成|截止|due/gi, '').trim();
        // 匹配 数字.数字 或 数字/数字 或 数字月数字日
        let dotMatch = cleaned.match(/(\d{1,2})[\.\/](\d{1,2})/);
        if (dotMatch) {
            let month = parseInt(dotMatch[1]);
            let day = parseInt(dotMatch[2]);
            if (month >= 1 && month <= 12 && day >= 1 && day <= 31) {
                return `${month}月${day}日`;
            }
        }
        let chnMatch = cleaned.match(/(\d{1,2})月(\d{1,2})日?/);
        if (chnMatch) {
            return `${chnMatch[1]}月${chnMatch[2]}日`;
        }
        // 部分只写 "7.1完成" 已经match过
        if (cleaned.length > 0 && /^\d{1,2}\.\d{1,2}$/.test(cleaned)) {
            let [m,d] = cleaned.split('.');
            return `${parseInt(m)}月${parseInt(d)}日`;
        }
        return cleaned;
    }
    
    // 添加待办列表(合并)
    function addTodosFromParsed(parsedArray) {
        if (!parsedArray.length) {
            showToast('未识别到有效的待办事项，请调整文本格式', 1800);
            return false;
        }
        let addedCount = 0;
        for (let p of parsedArray) {
            if (!p.task || (p.task.trim() === '' && !p.owner)) continue;
            const newId = Date.now() + Math.random() * 10000 + addedCount;
            todoItems.push({
                id: newId,
                task: p.task.substring(0, 100),
                owner: p.owner || '',
                deadline: p.deadline || '',
                raw: p
            });
            addedCount++;
        }
        if (addedCount > 0) {
            renderTodos();
            saveTodosToLocal();
            showToast(`✅ 成功解析并添加 ${addedCount} 项待办`);
            return true;
        } else {
            showToast('⚠️ 未能解析出结构化待办 (尝试“任务--负责人 日期”格式)', 2000);
            return false;
        }
    }
    
    // 从文本解析并追加
    function parseFromTextAndAdd(textContent) {
        if (!textContent || textContent.trim() === '') {
            showToast('请输入会议纪要文本', 1200);
            return;
        }
        const parsed = parseMeetingNotesToTodos(textContent);
        if (parsed.length) {
            addTodosFromParsed(parsed);
        } else {
            showToast('无法从文本中提取待办，请参考示例格式 (任务--负责人 7.1完成)', 2000);
        }
    }
    
    // 图片识别核心: 使用 Tesseract.js 进行 OCR (CDN)
    // 加载 Tesseract 库
    let tesseractLoaded = false;
    function loadTesseract() {
        return new Promise((resolve, reject) => {
            if (window.Tesseract) {
                resolve(window.Tesseract);
                return;
            }
            const script = document.createElement('script');
            script.src = 'https://cdn.jsdelivr.net/npm/tesseract.js@5/dist/tesseract.min.js';
            script.onload = () => {
                resolve(window.Tesseract);
            };
            script.onerror = () => reject(new Error('Tesseract加载失败，请检查网络'));
            document.head.appendChild(script);
        });
    }
    
    // 处理图片Blob/File 进行OCR
    async function recognizeImageAndParse(imageFileOrBlob) {
        const loadingToast = showToast('🔍 正在识别图片中的文字，请稍候...', 3000);
        try {
            const Tesseract = await loadTesseract();
            // 使用中文+英文语言包提高识别率
            const worker = await Tesseract.createWorker('chi_sim+eng');
            const ret = await worker.recognize(imageFileOrBlob);
            const recognizedText = ret.data.text;
            await worker.terminate();
            if (!recognizedText || recognizedText.trim() === '') {
                showToast('图片未识别出文字，请确保图片清晰或手动输入', 2000);
                return;
            }
            // 显示识别到的原始文本到文本域
            rawTextArea.value = recognizedText;
            // 自动解析识别文本
            const parsedTodos = parseMeetingNotesToTodos(recognizedText);
            if (parsedTodos.length) {
                addTodosFromParsed(parsedTodos);
                showToast(`📸 图片识别成功，解析出 ${parsedTodos.length} 条待办`, 2500);
            } else {
                showToast('图片文本未匹配到待办格式，已在编辑框展示原始内容，可手动调整后点击解析', 3000);
            }
        } catch (err) {
            console.error(err);
            showToast('OCR识别失败，请重试或手动输入', 2000);
        }
    }
    
    // 处理粘贴图片
    function handlePasteImage(e) {
        const items = e.clipboardData?.items;
        if (!items) return;
        for (let i = 0; i < items.length; i++) {
            if (items[i].type.indexOf('image') !== -1) {
                const blob = items[i].getAsFile();
                if (blob) {
                    // 预览
                    const url = URL.createObjectURL(blob);
                    previewImg.src = url;
                    previewImg.style.display = 'block';
                    recognizeImageAndParse(blob);
                    e.preventDefault();
                    return;
                }
            }
        }
        showToast('未检测到图片，请复制图片后按Ctrl+V', 1500);
    }
    
    // 点击粘贴区触发上传
    function triggerFileUpload() {
        const input = document.createElement('input');
        input.type = 'file';
        input.accept = 'image/*';
        input.onchange = (e) => {
            const file = e.target.files[0];
            if (file) {
                const url = URL.createObjectURL(file);
                previewImg.src = url;
                previewImg.style.display = 'block';
                recognizeImageAndParse(file);
            }
        };
        input.click();
    }
    
    // localStorage 持久化
    function saveTodosToLocal() {
        const store = todoItems.map(({ id, task, owner, deadline }) => ({ id, task, owner, deadline }));
        localStorage.setItem('meeting_todos', JSON.stringify(store));
    }
    
    function loadTodosFromLocal() {
        const stored = localStorage.getItem('meeting_todos');
        if (stored) {
            try {
                const arr = JSON.parse(stored);
                if (Array.isArray(arr) && arr.length) {
                    todoItems = arr;
                    renderTodos();
                }
            } catch(e) {}
        }
    }
    
    // 清空
    clearAllBtn.addEventListener('click', () => {
        if (todoItems.length && confirm('确认清空全部待办吗？')) {
            todoItems = [];
            renderTodos();
            saveTodosToLocal();
            showToast('已清空所有待办');
        } else if(todoItems.length === 0) {
            showToast('暂无待办可清空');
        }
    });
    
    parseTextBtn.addEventListener('click', () => {
        const manualText = rawTextArea.value;
        if (!manualText.trim()) {
            showToast('请在文本框输入会议纪要内容', 1200);
            return;
        }
        parseFromTextAndAdd(manualText);
    });
    
    // 监听全局粘贴
    window.addEventListener('paste', (e) => {
        // 只在当前页面生效 无额外限制
        handlePasteImage(e);
    });
    
    pasteZone.addEventListener('click', () => {
        triggerFileUpload();
    });
    
    // 初始化加载本地数据
    loadTodosFromLocal();
    
    // 初始示例预置一条示例 (可选 但让用户看到效果)
    if (todoItems.length === 0) {
        // 展示一个演示样例但不强制干扰用户，如果想展示可以取消注释，但为了干净体验不自动添加示例，而是显示提示 
        // 不过为了展示解析器功能，可以放一条示例数据（但用户可清空）
        // 友善: 放入一条示例解析帮助（但仅当为空时）
        const demoText = "笼筐清洗--GJL，7.1完成\n外包装袋油污优化--SK 7.3完成";
        const demoParsed = parseMeetingNotesToTodos(demoText);
        if (demoParsed.length && todoItems.length === 0) {
            demoParsed.forEach(p => {
                todoItems.push({
                    id: Date.now() + Math.random() * 100,
                    task: p.task,
                    owner: p.owner,
                    deadline: p.deadline,
                });
            });
            renderTodos();
            saveTodosToLocal();
        }
    }
</script>
</body>
</html>
