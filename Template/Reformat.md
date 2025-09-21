<%*
const clipboard = await tp.system.clipboard();

// Metni işle: paragrafları koru, satır içi satır sonlarını temizle
const marked = clipboard
    .replace(/\r?\n\r?\n/g, '<PARA>')  // Paragrafları geçici olarak işaretle
    .replace(/\r?\n/g, ' ')           // Normal satır sonlarını boşlukla değiştir
    .replace(/ +/g, ' ')              // Gereksiz fazla boşlukları temizle
    .replace(/<PARA>/g, '\n\n')       // Paragrafları geri getir
    .trim();

tR += marked;
%>
