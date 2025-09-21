<%*
// Obsidian Manual Toolbar Toggle (Ctrl+P ile çalıştır)
const pluginId = 'editing-toolbar';
const plugin = app.plugins.plugins[pluginId];

if (!plugin) {
    new Notice('❌ Editing Toolbar eklentisi bulunamadı!');
    return;
}

const currentSettings = plugin.settings;
const isAutoHideEnabled = currentSettings.autoHide;

// Ayarları tersine çevir
currentSettings.autoHide = !isAutoHideEnabled;
currentSettings.autohide = !isAutoHideEnabled;

try {
    await plugin.saveSettings();
    
    // Eklentiyi yeniden başlat
    await app.plugins.disablePlugin(pluginId);
    await app.plugins.enablePlugin(pluginId);
    
    // Eski stil varsa sil
    const styleId = "toolbar-complete-hide-style";
    const oldStyle = document.getElementById(styleId);
    if (oldStyle) oldStyle.remove();
    
    // CSS dosya yolları
    const cssFilePath = '.obsidian/snippets/toolbar-hide-permanent.css';
    const cssDisabledPath = '.obsidian/snippets/toolbar-hide-permanent.css.disabled';
    
    const css = `
        /* Tüm toolbar UI'sini tamamen kaldır */
        #editingToolbarModalBar,
        .editingToolbarDefaultAesthetic,
        .editing-toolbar,
        .editing-toolbar-container {
            display: none !important;
            visibility: hidden !important;
            opacity: 0 !important;
            pointer-events: none !important;
            height: 0 !important;
            overflow: hidden !important;
        }
        
        /* Hover efektlerini tamamen devre dışı bırak */
        .editing-toolbar:hover,
        .editing-toolbar:focus,
        .editing-toolbar:focus-within,
        .editing-toolbar:active,
        .workspace-leaf-content:hover .editing-toolbar,
        .workspace-leaf-content:focus .editing-toolbar,
        .view-header:hover + .view-content .editing-toolbar {
            display: none !important;
        }`;
    
    if (currentSettings.autoHide) {
        // Toolbar GİZLE
        const style = document.createElement("style");
        style.id = styleId;
        style.textContent = css;
        document.head.appendChild(style);
        
        // CSS dosyası yönetimi - akıllı yaklaşım
        const activeFileExists = await app.vault.adapter.exists(cssFilePath);
        const disabledFileExists = await app.vault.adapter.exists(cssDisabledPath);
        
        if (disabledFileExists && !activeFileExists) {
            // Devre dışı dosyayı aktive et (yeniden adlandır)
            await app.vault.adapter.rename(cssDisabledPath, cssFilePath);
        } else if (!activeFileExists && !disabledFileExists) {
            // Hiçbir dosya yok, yeni oluştur
            await app.vault.adapter.write(cssFilePath, css);
        } else if (activeFileExists) {
            // Aktif dosya var, içeriğini kontrol et
            try {
                const existingContent = await app.vault.adapter.read(cssFilePath);
                if (existingContent.trim() !== css.trim()) {
                    await app.vault.adapter.write(cssFilePath, css);
                } 
            } catch (readError) {
                // Okuma hatası varsa yeniden oluştur
                await app.vault.adapter.write(cssFilePath, css);
            }
        }
        
    } else {
        // Toolbar GÖSTER - Dosyayı sil değil, devre dışı bırak
        const activeFileExists = await app.vault.adapter.exists(cssFilePath);
        const disabledFileExists = await app.vault.adapter.exists(cssDisabledPath);
        
        if (activeFileExists && !disabledFileExists) {
            // Aktif dosyayı devre dışı yap (yeniden adlandır)
            await app.vault.adapter.rename(cssFilePath, cssDisabledPath);
        } else if (activeFileExists && disabledFileExists) {
            // Her iki dosya da var, aktifi sil
            await app.vault.adapter.remove(cssFilePath);
        }
    }
} catch (err) {
    new Notice('❌ İşlem sırasında hata oluştu: ' + err.message);
}
%>