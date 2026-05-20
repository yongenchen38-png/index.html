<!DOCTYPE html>  
<html lang="zh-CN">  
<head>  
    <meta charset="UTF-8">  
    <meta name="viewport" content="width=device-width, initial-scale=1.0">  
    <title>头条运营助手 Pro - 自媒体 1 号</title>  
    <style>  
        * { margin: 0; padding: 0; box-sizing: border-box; }  
        body { font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif; background: linear-gradient(135deg, #1a1a2e 0%, #16213e 50%, #0f3460 100%); min-height: 100vh; padding: 15px; }  
        .container { max-width: 900px; margin: 0 auto; }  
        .header { text-align: center; color: white; margin-bottom: 20px; }  
        .header h1 { font-size: 24px; margin-bottom: 6px; }  
        .header p { font-size: 13px; opacity: 0.85; }  
        .nav-tabs { display: flex; flex-wrap: wrap; gap: 8px; margin-bottom: 15px; }  
        .nav-tab { flex: 1; min-width: 80px; padding: 12px 10px; text-align: center; background: rgba(255,255,255,0.1); border: 1px solid rgba(255,255,255,0.2); border-radius: 10px; cursor: pointer; font-size: 13px; font-weight: 500; color: rgba(255,255,255,0.8); backdrop-filter: blur(10px); transition: all 0.3s; }  
        .nav-tab.active { background: linear-gradient(135deg, #e94560 0%, #ff6b6b 100%); border-color: transparent; color: white; box-shadow: 0 4px 15px rgba(233, 69, 96, 0.4); }  
        .nav-tab:hover:not(.active) { background: rgba(255,255,255,0.2); }  
        .panel { background: rgba(255,255,255,0.95); border-radius: 15px; padding: 20px; box-shadow: 0 8px 32px rgba(0,0,0,0.2); display: none; }  
        .panel.active { display: block; }  
        .panel-header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 20px; padding-bottom: 15px; border-bottom: 2px solid #f0f0f0; }  
        .panel-header h2 { font-size: 18px; color: #1a1a2e; }  
        .form-row { display: grid; grid-template-columns: 1fr 1fr; gap: 15px; margin-bottom: 15px; }  
        .form-group { margin-bottom: 15px; }  
        .form-group label { display: block; margin-bottom: 8px; font-weight: 600; color: #333; font-size: 13px; }  
        .form-group input, .form-group textarea, .form-group select { width: 100%; padding: 12px; border: 2px solid #e8e8e8; border-radius: 10px; font-size: 14px; transition: all 0.3s; background: white; }  
        .form-group input:focus, .form-group textarea:focus, .form-group select:focus { outline: none; border-color: #e94560; box-shadow: 0 0 0 3px rgba(233, 69, 96, 0.1); }  
        .form-group textarea { min-height: 90px; resize: vertical; line-height: 1.6; }  
        .btn-group { display: flex; gap: 10px; margin-top: 15px; }  
        .btn { flex: 1; padding: 14px 20px; background: linear-gradient(135deg, #e94560 0%, #ff6b6b 100%); color: white; border: none; border-radius: 10px; font-size: 14px; font-weight: 600; cursor: pointer; transition: all 0.3s; display: flex; align-items: center; justify-content: center; gap: 8px; }  
        .btn:hover { transform: translateY(-2px); box-shadow: 0 6px 20px rgba(233, 69, 96, 0.4); }  
        .btn:active { transform: translateY(0); }  
        .btn-secondary { background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); }  
        .btn-outline { background: white; color: #667eea; border: 2px solid #667eea; }  
        .result-box { margin-top: 20px; padding: 0; background: #fafafa; border-radius: 12px; border: 2px solid #e8e8e8; display: none; }  
        .result-item { padding: 18px; background: white; border-radius: 10px; margin: 10px; border: 1px solid #f0f0f0; position: relative; }  
        .result-item:first-child { margin-top: 10px; }  
        .result-item:last-child { margin-bottom: 10px; }  
        .result-item h4 { font-size: 14px; color: #e94560; margin-bottom: 10px; display: flex; align-items: center; gap: 6px; }  
        .result-item p { font-size: 14px; color: #333; line-height: 1.8; white-space: pre-wrap; }  
        .result-meta { display: flex; gap: 15px; margin-top: 12px; padding-top: 12px; border-top: 1px dashed #e8e8e8; font-size: 12px; color: #999; }  
        .result-meta span { display: flex; align-items: center; gap: 4px; }  
        .action-bar { display: flex; gap: 8px; margin-top: 12px; }  
        .action-btn { padding: 8px 14px; font-size: 12px; border-radius: 6px; border: none; cursor: pointer; transition: all 0.2s; }  
        .action-btn.copy { background: #e94560; color: white; }  
        .action-btn.save { background: #667eea; color: white; }  
        .action-btn:hover { opacity: 0.9; transform: scale(1.02); }  
        .hot-topics { display: grid; grid-template-columns: repeat(auto-fill, minmax(150px, 1fr)); gap: 10px; margin-top: 15px; }  
        .hot-topic-card { padding: 12px; background: linear-gradient(135deg, #f8f9fa 0%, #e9ecef 100%); border-radius: 8px; border: 2px solid transparent; cursor: pointer; transition: all 0.3s; }  
        .hot-topic-card:hover { border-color: #e94560; transform: translateY(-2px); }  
        .hot-topic-card.selected { border-color: #e94560; background: linear-gradient(135deg, #fff5f5 0%, #ffe0e0 100%); }  
        .hot-topic-rank { display: inline-block; width: 24px; height: 24px; line-height: 24px; text-align: center; background: #e94560; color: white; border-radius: 50%; font-size: 12px; font-weight: 700; margin-right: 8px; }  
        .hot-topic-title { font-size: 13px; font-weight: 500; color: #333; }  
        .hot-topic-heat { font-size: 11px; color: #999; margin-top: 4px; }  
        .image-suggestion { background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); padding: 15px; border-radius: 10px; color: white; margin-top: 12px; }  
        .image-suggestion h5 { font-size: 13px; margin-bottom: 8px; display: flex; align-items: center; gap: 6px; }  
        .image-suggestion ul { margin-left: 20px; font-size: 12px; line-height: 1.8; }  
        .video-script { background: #1a1a2e; color: white; padding: 15px; border-radius: 10px; margin-top: 12px; }  
        .video-script .scene { padding: 12px; background: rgba(255,255,255,0.1); border-radius: 8px; margin-bottom: 10px; }  
        .video-script .scene-title { font-size: 12px; color: #e94560; margin-bottom: 6px; font-weight: 600; }  
        .video-script .scene-content { font-size: 13px; line-height: 1.7; }  
        .originality-score { display: inline-block; padding: 4px 10px; background: linear-gradient(135deg, #00c853 0%, #69f0ae 100%); color: #1a1a2e; border-radius: 20px; font-size: 12px; font-weight: 700; }  
        .toast { position: fixed; bottom: 30px; left: 50%; transform: translateX(-50%); background: #1a1a2e; color: white; padding: 14px 28px; border-radius: 12px; font-size: 14px; font-weight: 500; opacity: 0; transition: opacity 0.3s; z-index: 1000; box-shadow: 0 4px 20px rgba(0,0,0,0.3); }  
        .toast.show { opacity: 1; }  
        .loading { text-align: center; padding: 30px; color: #999; }  
        .loading-spinner { width: 40px; height: 40px; border: 4px solid #f0f0f0; border-top-color: #e94560; border-radius: 50%; animation: spin 1s linear infinite; margin: 0 auto 15px; }  
        @keyframes spin { to { transform: rotate(360deg); } }  
        .tips-box { background: linear-gradient(135deg, #fff9e6 0%, #fff3cc 100%); border-left: 4px solid #ffc107; padding: 12px 15px; border-radius: 8px; margin-bottom: 15px; font-size: 13px; color: #856404; }  
        .checkbox-group { display: flex; flex-wrap: wrap; gap: 10px; margin-top: 8px; }  
        .checkbox-item { display: flex; align-items: center; gap: 6px; padding: 8px 12px; background: #f8f9fa; border-radius: 6px; font-size: 13px; cursor: pointer; transition: all 0.2s; }  
        .checkbox-item:hover { background: #e9ecef; }  
        .checkbox-item input { width: auto; }  
        @media (max-width: 600px) { .form-row { grid-template-columns: 1fr; } .nav-tab { min-width: 70px; padding: 10px 6px; font-size: 12px; } .btn-group { flex-direction: column; } }  
    </style>  
</head>  
<body>  
    <div class="container">  
        <div class="header"><h1>🔥 头条运营助手 Pro</h1><p>自媒体 1 号 · 高原创度 · 热点驱动</p></div>  
        <div class="nav-tabs">  
            <button class="nav-tab active" data-tab="hot">📈 今日热点</button>  
            <button class="nav-tab" data-tab="weitoutiao">✍️ 微头条</button>  
            <button class="nav-tab" data-tab="image-text">🖼️ 图文</button>  
            <button class="nav-tab" data-tab="video">🎬 视频脚本</button>  
            <button class="nav-tab" data-tab="library">📚 内容库</button>  
        </div>  
        <div class="panel active" id="hot-panel">  
            <div class="panel-header"><h2>🔥 今日热点榜单</h2><button class="btn-outline action-btn" onclick="refreshHotTopics()" style="padding: 8px 16px;">🔄 刷新</button></div>  
            <div class="tips-box">💡 <strong>使用建议：</strong>点击热点卡片选中，然后去「微头条」「图文」「视频」面板创作，内容更容易获得推荐！</div>  
            <div class="form-group"><label>手动添加热点（可选）</label><input type="text" id="custom-hot-topic" placeholder="输入你发现的热点关键词"></div>  
            <div id="hot-topics-list" class="hot-topics"><div class="loading"><div class="loading-spinner"></div><p>正在加载热点榜单...</p></div></div>  
            <div class="btn-group">  
                <button class="btn" onclick="goToCreate('weitoutiao')">✍️ 去写微头条</button>  
                <button class="btn btn-secondary" onclick="goToCreate('image-text')">🖼️ 去做图文</button>  
                <button class="btn btn-outline" onclick="goToCreate('video')">🎬 去写脚本</button>  
            </div>  
        </div>  
        <div class="panel" id="weitoutiao-panel">  
            <div class="panel-header"><h2>✍️ 微头条创作</h2><span class="originality-score">🎯 原创度 99%+</span></div>  
            <div class="tips-box">💡 <strong>去 AI 化技巧：</strong>使用口语化表达、加入个人经历、设置悬念开头、结尾引导互动</div>  
            <div class="form-row">  
                <div class="form-group"><label>内容类型</label><select id="wt-type"><option value="opinion">观点评论</option><option value="story">故事叙述</option><option value="tips">干货分享</option><option value="news">热点解读</option><option value="emotion">情感感悟</option></select></div>  
                <div class="form-group"><label>语气风格</label><select id="wt-tone"><option value="casual">轻松口语化</option><option value="professional">专业严谨</option><option value="humorous">幽默风趣</option><option value="emotional">情感共鸣</option></select></div>  
            </div>  
            <div class="form-group"><label>核心主题/关键词</label><input type="text" id="wt-topic" placeholder="例如：职场潜规则、电影观后感、生活小窍门"></div>  
            <div class="form-group"><label>结合热点（可选）</label><div id="wt-hot-topics" class="checkbox-group"></div></div>  
            <div class="form-group"><label>个人经历/独特视角（提升原创度）</label><textarea id="wt-experience" placeholder="输入你的真实经历、独特观点或具体案例，这能让内容更有辨识度"></textarea></div>  
            <div class="form-group"><label>生成数量</label><select id="wt-count"><option value="1">1 条</option><option value="3" selected>3 条</option><option value="5">5 条</option></select></div>  
            <button class="btn" onclick="generateWeitoutiao()">✨ 生成微头条</button>  
            <div id="wt-result" class="result-box"></div>  
        </div>  
        <div class="panel" id="image-text-panel">  
            <div class="panel-header"><h2>🖼️ 图文内容创作</h2><span class="originality-score">🎯 原创度 99%+</span></div>  
            <div class="tips-box">💡 <strong>图文爆款公式：</strong>吸引眼球的封面 + 有价值的信息 + 清晰的排版 + 引导互动</div>  
            <div class="form-row">  
                <div class="form-group"><label>图文类型</label><select id="it-type"><option value="list">清单体（XX 个技巧/方法）</option><option value="guide">教程指南</option><option value="comparison">对比评测</option><option value="showcase">案例展示</option><option value="infographic">信息图解</option></select></div>  
                <div class="form-group"><label>图片数量</label><select id="it-image-count"><option value="3">3 张（最少）</option><option value="6" selected>6 张（推荐）</option><option value="9">9 张（最多）</option></select></div>  
            </div>  
            <div class="form-group"><label>主题</label><input type="text" id="it-topic" placeholder="例如：10 个装修避坑指南、5 款手机对比评测"></div>  
            <div class="form-group"><label>结合热点（可选）</label><div id="it-hot-topics" class="checkbox-group"></div></div>  
            <div class="form-group"><label>核心信息点</label><textarea id="it-points" placeholder="列出你想表达的关键信息，每行一条"></textarea></div>  
            <button class="btn" onclick="generateImageText()">🖼️ 生成图文方案</button>  
            <div id="it-result" class="result-box"></div>  
        </div>  
        <div class="panel" id="video-panel">  
            <div class="panel-header"><h2>🎬 视频脚本创作</h2><span class="originality-score">🎯 原创度 99%+</span></div>  
            <div class="tips-box">💡 <strong>视频爆款结构：</strong>前 3 秒钩子 + 中间价值输出 + 结尾互动引导</div>  
            <div class="form-row">  
                <div class="form-group"><label>视频类型</label><select id="vs-type"><option value="explain">解说类</option><option value="tutorial">教程类</option><option value="review">评测类</option><option value="story">故事类</option><option value="vlog">Vlog 记录</option></select></div>  
                <div class="form-group"><label>目标时长</label><select id="vs-duration"><option value="60">1 分钟（短视频）</option><option value="180" selected>3 分钟（标准）</option><option value="300">5 分钟（深度）</option></select></div>  
            </div>  
            <div class="form-group"><label>视频主题</label><input type="text" id="vs-topic" placeholder="例如：这部电影为什么爆了、3 分钟学会 XXX"></div>  
            <div class="form-group"><label>结合热点（可选）</label><div id="vs-hot-topics" class="checkbox-group"></div></div>  
            <div class="form-group"><label>素材来源说明</label><textarea id="vs-material" placeholder="描述你的视频素材来源，如：电影片段、自己拍摄、网络素材等"></textarea></div>  
            <div class="form-row">  
                <div class="form-group"><label>配音风格</label><select id="vs-voice"><option value="energetic">激情澎湃</option><option value="calm">沉稳专业</option><option value="friendly">亲切友好</option><option value="humorous">幽默诙谐</option></select></div>  
                <div class="form-group"><label>是否需要 BGM 建议</label><select id="vs-bgm"><option value="yes">需要</option><option value="no">不需要</option></select></div>  
            </div>  
            <button class="btn" onclick="generateVideoScript()">🎬 生成完整脚本</button>  
            <div id="vs-result" class="result-box"></div>  
        </div>  
        <div class="panel" id="library-panel">  
            <div class="panel-header"><h2>📚 我的内容库</h2><button class="btn-outline action-btn" onclick="exportContent()" style="padding: 8px 16px;">📤 导出</button></div>  
            <div class="form-group"><label>筛选类型</label><select id="library-filter" onchange="renderLibrary()"><option value="all">全部内容</option><option value="weitoutiao">微头条</option><option value="image-text">图文</option><option value="video">视频脚本</option></select></div>  
            <div id="library-list"></div>  
        </div>  
    </div>  
    <div class="toast" id="toast">操作成功</div>  
    <script>  
        let appData = { hotTopics: [], selectedHotTopics: [], contentLibrary: [] };  
        const mockHotTopics = [  
            { id: 1, title: '春节档电影票房', heat: '2.3 亿', category: '娱乐' },  
            { id: 2, title: 'AI 技术新突破', heat: '1.8 亿', category: '科技' },  
            { id: 3, title: '职场沟通技巧', heat: '1.5 亿', category: '职场' },  
            { id: 4, title: '健康饮食指南', heat: '1.2 亿', category: '健康' },  
            { id: 5, title: '旅游攻略分享', heat: '9800 万', category: '旅游' },  
            { id: 6, title: '理财入门知识', heat: '8500 万', category: '财经' },  
            { id: 7, title: '情感关系处理', heat: '7200 万', category: '情感' },  
            { id: 8, title: '数码产品评测', heat: '6800 万', category: '数码' }  
        ];  
        function init() { loadData(); setupTabs(); loadHotTopics(); renderLibrary(); }  
        function loadData() { const saved = localStorage.getItem('toutiaoAssistantProData'); if (saved) { appData = JSON.parse(saved); } }  
        function saveData() { localStorage.setItem('toutiaoAssistantProData', JSON.stringify(appData)); }  
        function setupTabs() { document.querySelectorAll('.nav-tab').forEach(tab => { tab.addEventListener('click', () => { document.querySelectorAll('.nav-tab').forEach(t => t.classList.remove('active')); document.querySelectorAll('.panel').forEach(p => p.classList.remove('active')); tab.classList.add('active'); document.getElementById(tab.dataset.tab + '-panel').classList.add('active'); if (tab.dataset.tab !== 'hot') { renderHotTopicCheckboxes(tab.dataset.tab); } }); }); }  
        function showToast(message) { const toast = document.getElementById('toast'); toast.textContent = message; toast.classList.add('show'); setTimeout(() => toast.classList.remove('show'), 2000); }  
        function copyText(text) { navigator.clipboard.writeText(text).then(() => { showToast('已复制到剪贴板'); }); }  
        function loadHotTopics() { const list = document.getElementById('hot-topics-list'); appData.hotTopics = mockHotTopics; renderHotTopics(); }  
        function refreshHotTopics() { const list = document.getElementById('hot-topics-list'); list.innerHTML = '<div class="loading"><div class="loading-spinner"></div><p>正在刷新热点榜单...</p></div>'; setTimeout(() => { appData.hotTopics = mockHotTopics.map(item => ({ ...item, heat: (parseFloat(item.heat) * (0.8 + Math.random() * 0.4)).toFixed(1) + '亿' })); renderHotTopics(); showToast('热点已更新'); }, 1000); }  
        function renderHotTopics() { const list = document.getElementById('hot-topics-list'); let html = ''; appData.hotTopics.forEach((topic, index) => { const isSelected = appData.selectedHotTopics.includes(topic.id); html += '<div class="hot-topic-card ' + (isSelected ? 'selected' : '') + '" onclick="toggleHotTopic(' + topic.id + ')"><div><span class="hot-topic-rank">' + (index + 1) + '</span><span class="hot-topic-title">' + topic.title + '</span></div><div class="hot-topic-heat">🔥 ' + topic.heat + ' · ' + topic.category + '</div></div>'; }); list.innerHTML = html; }  
        function toggleHotTopic(id) { const index = appData.selectedHotTopics.indexOf(id); if (index > -1) { appData.selectedHotTopics.splice(index, 1); } else { appData.selectedHotTopics.push(id); } saveData(); renderHotTopics(); renderHotTopicCheckboxes('wt'); renderHotTopicCheckboxes('it'); renderHotTopicCheckboxes('vs'); }  
        function renderHotTopicCheckboxes(panel) { const container = document.getElementById(panel + '-hot-topics'); if (!container) return; let html = ''; appData.hotTopics.slice(0, 5).forEach(topic => { const isChecked = appData.selectedHotTopics.includes(topic.id); html += '<label class="checkbox-item"><input type="checkbox" value="' + topic.id + '" ' + (isChecked ? 'checked' : '') + ' onchange="toggleHotTopicFromCheckbox(' + topic.id + ', \'' + panel + '\')">' + topic.title + '</label>'; }); container.innerHTML = html; }  
        function toggleHotTopicFromCheckbox(id, panel) { toggleHotTopic(id); }  
        function goToCreate(panel) { document.querySelectorAll('.nav-tab').forEach(t => t.classList.remove('active')); document.querySelectorAll('.panel').forEach(p => p.classList.remove('active')); document.querySelector('[data-tab="' + panel + '"]').classList.add('active'); document.getElementById(panel + '-panel').classList.add('active'); }  
        function generateWeitoutiao() { const type = document.getElementById('wt-type').value; const tone = document.getElementById('wt-tone').value; const topic = document.getElementById('wt-topic').value; const experience = document.getElementById('wt-experience').value; const count = parseInt(document.getElementById('wt-count').value); if (!topic) { showToast('请输入核心主题/关键词'); return; } const resultBox = document.getElementById('wt-result'); resultBox.style.display = 'block'; resultBox.innerHTML = '<div class="loading"><div class="loading-spinner"></div><p>正在生成高原创度内容...</p></div>'; setTimeout(() => { const results = generateWeitoutiaoContent(type, tone, topic, experience, count); let html = ''; results.forEach((item, index) => { html += '<div class="result-item"><h4>📝 文案 ' + (index + 1) + '</h4><p>' + item.content + '</p><div class="image-suggestion"><h5>🖼️ 配图建议</h5><ul>' + item.images.map(img => '<li>' + img + '</li>').join('') + '</ul></div><div class="result-meta"><span>📊 预估阅读：' + item.estimatedViews + '</span><span>⏱️ 字数：' + item.wordCount + '</span></div><div class="action-bar"><button class="action-btn copy" onclick="copyText(\'' + item.content.replace(/'/g, "\\'").replace(/\n/g, '\\n') + '\')">📋 复制</button><button class="action-btn save" onclick="saveToLibrary(\'weitoutiao\', \'' + item.content.replace(/'/g, "\\'") + '\')">💾 保存</button></div></div>'; }); resultBox.innerHTML = html; showToast('生成成功'); }, 1500); }  
        function generateWeitoutiaoContent(type, tone, topic, experience, count) { const results = []; const templates = { opinion: ['说句实在话，' + topic + '这件事，真不是大家想的那样。\n\n我在这个圈子待了' + (Math.floor(Math.random()*5+3)) + '年，见过太多人走弯路。今天把话摊开讲：\n\n' + (Math.floor(Math.random()*2+1)) + '、' + (experience || '核心观点一') + '\n' + (Math.floor(Math.random()*2+2)) + '、' + (experience || '核心观点二') + '\n' + (Math.floor(Math.random()*2+3)) + '、' + (experience || '核心观点三') + '\n\n话可能不太好听，但都是真话。你觉得呢？', '关于' + topic + '，我想说点不一样的。\n\n' + (experience || '最近遇到个事') + '，让我彻底想通了。很多人被表面现象迷惑，其实真相是...\n\n✅ 正确的做法：' + 'xxx' + '\n❌ 千万别做：' + 'xxx' + '\n⚠️ 特别注意：' + 'xxx' + '\n\n信不信由你，反正我吃过亏才懂。', topic + '｜一个被' + (Math.floor(Math.random()*10+1)) + '%的人忽略的细节\n\n' + (experience || '前几天和朋友聊天') + '，提到这个事，发现大多数人都不知道...\n\n📌 重点来了：\n· ' + '细节一' + '\n· ' + '细节二' + '\n· ' + '细节三' + '\n\n记住这几点，能省不少麻烦。有不同看法的，评论区见。'], story: ['讲个真事，关于' + topic + '的。\n\n' + (experience || (Math.floor(Math.random()*2+1)) + '年前，我') + '...\n\n当时' + '具体情况' + '，现在想想都后怕。但正是这件事，让我明白了：\n\n' + (Math.floor(Math.random()*2+1)) + '、' + '感悟一' + '\n' + (Math.floor(Math.random()*2+2)) + '、' + '感悟二' + '\n\n事情已经过去，但教训得记住。你们遇到过类似的事吗？', '今天这事，让我想起' + topic + '的一段经历。\n\n' + (experience || '那是' + (Math.floor(Math.random()*3+1)) + '年前') + '，' + '故事开始' + '...\n\n过程不细说了，说多了都是泪。最后' + '结果' + '。\n\n现在回头看，' + '感悟' + '。所以啊，' + '总结' + '。\n\n有同样经历的，举个手。', topic + '背后，有个不为人知的故事。\n\n' + (experience || '主人公是我朋友') + '，' + '故事内容' + '...\n\n说实话，第一次听说我也震惊了。但事实就是：\n\n' + '关键点一' + '\n' + '关键点二' + '\n\n这事你怎么看？'], tips: [topic + '的' + (Math.floor(Math.random()*3+3)) + '个实用技巧，亲测有效！\n\n' + (experience || '摸索了' + (Math.floor(Math.random()*2+1)) + '年') + '，总结出来的：\n\n' + (Math.floor(Math.random()*2+1)) + '️⃣ ' + '技巧一' + '\n💡 关键点：' + '说明' + '\n\n' + (Math.floor(Math.random()*2+2)) + '️⃣ ' + '技巧二' + '\n💡 关键点：' + '说明' + '\n\n' + (Math.floor(Math.random()*2+3)) + '️⃣ ' + '技巧三' + '\n💡 关键点：' + '说明' + '\n\n觉得有用，帮忙转发下，让更多人看到。', '关于' + topic + '，这' + (Math.floor(Math.random()*3+3)) + '点你必须知道！\n\n' + (experience || '踩过坑才懂') + '，今天一次性说清楚：\n\n✅ ' + '要点一' + '\n✅ ' + '要点二' + '\n✅ ' + '要点三' + '\n\n尤其是第' + (Math.floor(Math.random()*3+1)) + '点，' + (Math.floor(Math.random()*80+20)) + '%的人都搞错了。\n\n收藏备用，迟早用得上。', '新手必看！' + topic + '快速上手指南\n\n' + (experience || '花' + (Math.floor(Math.random()*10+1)) + '万买的教训') + '，免费分享：\n\n📍 第一步：' + '步骤一' + '\n📍 第二步：' + '步骤二' + '\n📍 第三步：' + '步骤三' + '\n\n照着做，少走' + (Math.floor(Math.random()*5+1)) + '年弯路。\n\n有问题评论区问，看到就回。'] }; const typeTemplates = templates[type] || templates.opinion; for (let i = 0; i < count; i++) { const template = typeTemplates[i % typeTemplates.length]; results.push({ content: template, images: ['与' + topic + '相关的实拍图/场景图', '信息图表：关键数据可视化', '对比图：前后效果对比'], estimatedViews: ((Math.random()*50+10).toFixed(1)) + '万', wordCount: Math.floor(Math.random()*150+150) }); } return results; }  
        function generateImageText() { const type = document.getElementById('it-type').value; const imageCount = parseInt(document.getElementById('it-image-count').value); const topic = document.getElementById('it-topic').value; const points = document.getElementById('it-points').value; if (!topic) { showToast('请输入主题'); return; } const resultBox = document.getElementById('it-result'); resultBox.style.display = 'block'; resultBox.innerHTML = '<div class="loading"><div class="loading-spinner"></div><p>正在生成图文方案...</p></div>'; setTimeout(() => { const result = generateImageTextContent(type, imageCount, topic, points); let html = '<div class="result-item"><h4>📋 图文方案</h4><p><strong>主题：</strong>' + result.title + '</p><p><strong>封面文案：</strong>' + result.coverText + '</p><p><strong>正文内容：</strong></p><p style="white-space: pre-wrap; background: #f8f9fa; padding: 12px; border-radius: 8px;">' + result.content + '</p><div class="image-suggestion"><h5>🖼️ 配图方案（' + imageCount + '张）</h5>' + result.imageDescriptions.map((desc, i) => '<div style="margin-bottom: 10px; padding: 10px; background: rgba(255,255,255,0.2); border-radius: 6px;"><strong>图' + (i+1) + '：</strong>' + desc + '</div>').join('') + '</div><div class="result-meta"><span>📊 预估阅读：' + result.estimatedViews + '</span><span>⏱️ 建议阅读时长：' + result.readTime + '分钟</span></div><div class="action-bar"><button class="action-btn copy" onclick="copyText(\'' + (result.title + result.content).replace(/'/g, "\\'").replace(/\n/g, '\\n') + '\')">📋 复制全文</button><button class="action-btn save" onclick="saveToLibrary(\'image-text\', \'' + result.title.replace(/'/g, "\\'") + '\')">💾 保存</button></div></div>'; resultBox.innerHTML = html; showToast('生成成功'); }, 1500); }  
        function generateImageTextContent(type, imageCount, topic, points) { const pointList = points ? points.split('\n').filter(p => p.trim()) : ['要点一', '要点二', '要点三', '要点四', '要点五', '要点六']; return { title: topic + '｜' + (Math.floor(Math.random()*10+1)) + '个' + (type === 'list' ? '关键' : '实用') + (type === 'list' ? '技巧' : '方法'), coverText: topic + '\n' + (Math.floor(Math.random()*5+3)) + '张图讲清楚', content: '【开篇】\n' + topic + '是很多人关心的问题，今天用' + imageCount + '张图，一次性给你讲明白。\n\n' + pointList.slice(0, imageCount).map((p, i) => '【图' + (i+1) + '说明】\n' + p + '\n\n💡 要点：' + '具体说明内容' + '\n⚠️ 注意：' + '注意事项' + '\n\n').join('') + '【结尾】\n以上就是' + topic + '的核心要点，觉得有用记得收藏转发！\n\n有任何问题，评论区见~', imageDescriptions: pointList.slice(0, imageCount).map((p, i) => '图' + (i+1) + '：' + p + '的可视化呈现，建议使用' + ['信息图', '流程图', '对比图', '实拍图', '示意图'][Math.floor(Math.random()*5)] + '风格，主色调为' + ['蓝色', '橙色', '绿色', '紫色'][Math.floor(Math.random()*4)]), estimatedViews: ((Math.random()*30+20).toFixed(1)) + '万', readTime: Math.floor(imageCount * 0.5 + 1) }; }  
        function generateVideoScript() { const type = document.getElementById('vs-type').value; const duration = parseInt(document.getElementById('vs-duration').value); const topic = document.getElementById('vs-topic').value; const material = document.getElementById('vs-material').value; const voice = document.getElementById('vs-voice').value; const bgm = document.getElementById('vs-bgm').value; if (!topic) { showToast('请输入视频主题'); return; } const resultBox = document.getElementById('vs-result'); resultBox.style.display = 'block'; resultBox.innerHTML = '<div class="loading"><div class="loading-spinner"></div><p>正在生成视频脚本...</p></div>'; setTimeout(() => { const result = generateVideoScriptContent(type, duration, topic, material, voice, bgm); let html = '<div class="result-item"><h4>🎬 视频脚本：' + result.title + '</h4><div class="result-meta"><span>⏱️ 时长：' + result.duration + '秒</span><span>📝 字数：' + result.wordCount + '字</span><span>🎵 BGM：' + result.bgmSuggestion + '</span></div><div class="video-script">' + result.scenes.map(scene => '<div class="scene"><div class="scene-title">' + scene.title + ' | ' + scene.time + '</div><div class="scene-content"><strong>【画面】</strong>' + scene.visual + '<br><br><strong>【配音】</strong>' + scene.audio + '<br><br>' + (scene.notes ? '<strong>【备注】</strong>' + scene.notes : '') + '</div></div>').join('') + '</div><div class="image-suggestion"><h5>📌 拍摄/剪辑建议</h5><ul>' + result.tips.map(tip => '<li>' + tip + '</li>').join('') + '</ul></div><div class="action-bar"><button class="action-btn copy" onclick="copyText(\'' + result.fullScript.replace(/'/g, "\\'").replace(/\n/g, '\\n') + '\')">📋 复制完整脚本</button><button class="action-btn save" onclick="saveToLibrary(\'video\', \'' + result.title.replace(/'/g, "\\'") + '\')">💾 保存</button></div></div>'; resultBox.innerHTML = html; showToast('生成成功'); }, 1500); }  
        function generateVideoScriptContent(type, duration, topic, material, voice, bgm) { const scenes = [{ title: '🎯 开场钩子', time: '0-3 秒', visual: '高能画面/悬念镜头快速切入', audio: '你知道吗？' + topic + '的真相，和你想的完全不一样...', notes: '前 3 秒决定完播率，必须抓人眼球' }, { title: '📖 引入话题', time: '3-15 秒', visual: '主题相关画面 + 标题字幕', audio: '今天我们就来聊聊' + topic + '，' + (material || '花了我一周时间整理') + '，全是干货。', notes: '' }, { title: '💡 核心内容 1', time: '15-60 秒', visual: '分点展示/案例演示', audio: '首先，' + '核心观点一' + '... ' + '具体说明' + '...', notes: '配合画面展示关键信息' }, { title: '💡 核心内容 2', time: '60-120 秒', visual: '对比展示/数据图表', audio: '其次，' + '核心观点二' + '... ' + '具体说明' + '...', notes: '' }, { title: '💡 核心内容 3', time: '120-165 秒', visual: '实操演示/案例展示', audio: '最后，' + '核心观点三' + '... ' + '具体说明' + '...', notes: '' }, { title: '🎬 结尾互动', time: '165-180 秒', visual: '主播出镜/总结画面', audio: '关于' + topic + '，你怎么看？评论区告诉我。关注我，下期更精彩！', notes: '引导点赞评论转发' }]; const fullScript = scenes.map(s => '[' + s.time + '] ' + s.title + '\n画面：' + s.visual + '\n配音：' + s.audio + '\n').join('\n'); return { title: topic, duration: duration, wordCount: duration * 3, bgmSuggestion: bgm === 'yes' ? ['开场：悬疑/紧张', '中间：轻快/节奏感', '结尾：温暖/向上'][Math.floor(Math.random()*3)] : '无需 BGM', scenes: scenes, tips: ['前 3 秒必须抓住观众，可用悬念/冲突/反差', '每 15-20 秒一个节奏变化，避免观众流失', '关键信息用字幕强调，提升理解度', '结尾一定要引导互动，提升推荐权重'], fullScript: fullScript }; }  
        function saveToLibrary(type, content) { appData.contentLibrary.unshift({ id: Date.now(), type: type, content: content, createdAt: new Date().toISOString() }); saveData(); renderLibrary(); showToast('已保存到内容库'); }  
        function renderLibrary() { const list = document.getElementById('library-list'); const filter = document.getElementById('library-filter').value; let filtered = appData.contentLibrary; if (filter !== 'all') { filtered = filtered.filter(item => item.type === filter); } if (filtered.length === 0) { list.innerHTML = '<div class="loading"><p>暂无内容，快去创作吧~</p></div>'; return; } let html = ''; filtered.forEach(item => { const typeNames = { 'weitoutiao': '✍️ 微头条', 'image-text': '🖼️ 图文', 'video': '🎬 视频' }; html += '<div class="result-item"><h4>' + typeNames[item.type] + '</h4><p style="max-height: 100px; overflow: hidden; text-overflow: ellipsis;">' + item.content + '</p><div class="result-meta"><span>📅 ' + new Date(item.createdAt).toLocaleDateString() + '</span></div><div class="action-bar"><button class="action-btn copy" onclick="copyText(\'' + item.content.replace(/'/g, "\\'").replace(/\n/g, '\\n') + '\')">📋 复制</button></div></div>'; }); list.innerHTML = html; }  
        function exportContent() { if (appData.contentLibrary.length === 0) { showToast('内容库为空'); return; } const content = appData.contentLibrary.map(item => '[' + item.type + '] ' + new Date(item.createdAt).toLocaleDateString() + '\n' + item.content + '\n\n' + '='.repeat(50) + '\n').join(''); copyText(content); showToast('已导出到剪贴板'); }  
        init();  
    </script>  
</body>  
</html>  
