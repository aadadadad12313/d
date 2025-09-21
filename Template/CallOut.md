<%*
const callouts = {
  note: '🔵 ✏ Note',
  info: '🔵 i Info',
  todo: '🔵 🔳 Todo',
  tip: '🌐 🔥 Tip / Hint / Important',
  abstract: '🌐 📋 Abstract / Summary / TLDR',
  question: '🟡 ❓ Question / Help / FAQ',
  quote: '🔘 💬 Quote / Cite',
  example: '🟣 📑 Example',
  success: '🟢 ✔ Success / Check / Done',
  warning: '🟠 ⚠ Warning / Caution / Attention',
  failure: '🔴 ❌ Failure / Fail / Missing',
  danger: '🔴 ⚡ Danger / Error',
  bug: '🔴 🐞 Bug',
  cornflower: '🔷 🌼 Cornflower Blue'
};

const type = await tp.system.suggester(Object.values(callouts), Object.keys(callouts), true, 'Callout tipini seç:');
if (!type) return;

const title = await tp.system.prompt('Callout başlığı:', '', true);
if (!title) return;

const Modal = tp.obsidian.Modal;

class ContentModal extends Modal {
  constructor(app, onSubmit) {
    super(app);
    this.onSubmit = onSubmit;
    this.result = '';
    this.selectedState = '-'; // Varsayılan olarak kapalı
    this.selectedLanguage = ''; // Varsayılan olarak metin
  }

  onOpen() {
    const { contentEl } = this;
    this.modalEl.classList.add("callout-modal");
    
    // Başlık ve dropdown'ları aynı satıra koy
    const headerContainer = contentEl.createEl("div");
    headerContainer.style.display = "flex";
    headerContainer.style.alignItems = "center";
    headerContainer.style.justifyContent = "space-between";
    headerContainer.style.marginTop = "15px";
    headerContainer.style.marginBottom = "20px";

    const title = headerContainer.createEl("h2", { text: "Callout İçeriği" });
    title.style.margin = "0";

    // Dropdown'lar için container
    const dropdownContainer = headerContainer.createEl("div");
    dropdownContainer.style.display = "flex";
    dropdownContainer.style.gap = "10px";
    dropdownContainer.style.alignItems = "center";

    // Dil seçici - sol tarafta
    const languageSelect = dropdownContainer.createEl("select");
    languageSelect.style.padding = "6px 12px";
    languageSelect.style.borderRadius = "6px";
    languageSelect.style.border = "1px solid rgba(100, 148, 237, 0.76)";
    languageSelect.style.background = "rgba(25, 25, 35, 0.9)";
    languageSelect.style.color = "var(--text-normal)";
    languageSelect.style.fontSize = "13px";
    languageSelect.style.minWidth = "100px";

    // Dil seçenekleri
    const languageOptions = [
      { value: '', text: 'Metin' },
      { value: 'css', text: 'CSS' },
      { value: 'js', text: 'JavaScript' }
    ];

    languageOptions.forEach(option => {
      const optEl = languageSelect.createEl("option", { text: option.text });
      optEl.value = option.value;
      if (option.value === '') {
        optEl.selected = true;
      }
    });

    languageSelect.addEventListener('change', (e) => {
      this.selectedLanguage = e.target.value;
    });

    // Callout durumu seçici - sağ tarafta
    const stateSelect = dropdownContainer.createEl("select");
    stateSelect.style.padding = "6px 12px";
    stateSelect.style.borderRadius = "6px";
    stateSelect.style.border = "1px solid rgba(100, 148, 237, 0.76)";
    stateSelect.style.background = "rgba(25, 25, 35, 0.9)";
    stateSelect.style.color = "var(--text-normal)";
    stateSelect.style.fontSize = "13px";
    stateSelect.style.minWidth = "130px";

    // Durum seçenekleri
    const stateOptions = [
      { value: '-', text: 'Kapalı (-)' },
      { value: '+', text: 'Açık (+)' },
      { value: '', text: 'Sürekli Açık' }
    ];

    stateOptions.forEach(option => {
      const optEl = stateSelect.createEl("option", { text: option.text });
      optEl.value = option.value;
      if (option.value === '-') {
        optEl.selected = true;
      }
    });

    stateSelect.addEventListener('change', (e) => {
      this.selectedState = e.target.value;
    });

    const desc = contentEl.createEl("p");
    desc.style.color = "var(--text-muted)";

    this.textArea = contentEl.createEl("textarea");
    this.textArea.placeholder = "İçeriğinizi yazın...";
    this.textArea.style.width = "100%";
    this.textArea.style.height = "200px";
    this.textArea.style.padding = "14px";
    this.textArea.style.fontFamily = '"Fira Code", monospace';
    this.textArea.style.fontSize = "14px";
    this.textArea.style.lineHeight = "1.6";
    this.textArea.style.color = "#e6f1ff";
    this.textArea.style.background = "rgba(25, 25, 35, 0.9)";
    this.textArea.style.border = "1px solid rgba(100, 148, 237, 0.76)";
    this.textArea.style.borderRadius = "10px";
    this.textArea.style.resize = "vertical";
    this.textArea.style.boxShadow = "inset 0 0 6px rgba(0, 200, 255, 0.15)";

    const buttonDiv = contentEl.createEl("div");
    buttonDiv.style.marginTop = "10px";
    buttonDiv.style.textAlign = "right";

    const submitBtn = buttonDiv.createEl("button", { text: "Tamam" });
    submitBtn.className = "callout-submit-btn";
    submitBtn.onclick = () => {
      this.result = {
        content: this.textArea.value,
        state: this.selectedState,
        language: this.selectedLanguage
      };
      this.close();
    };

    this.textArea.addEventListener('keydown', (e) => {
      if (e.ctrlKey && e.key === 'Enter') {
        e.preventDefault();
        this.result = {
          content: this.textArea.value,
          state: this.selectedState,
          language: this.selectedLanguage
        };
        this.close();
      }
    });

    // İlk yükleme için placeholder'ı ayarla
    this.updateTextAreaPlaceholder();
    
    setTimeout(() => this.textArea.focus(), 100);
  }

  updateTextAreaPlaceholder() {
    if (this.selectedLanguage === 'css') {
      this.textArea.placeholder = "CSS kodunuzu yazın...\n\n.example {\n  color: #333;\n  font-size: 16px;\n}";
    } else if (this.selectedLanguage === 'js') {
      this.textArea.placeholder = "JavaScript kodunuzu yazın...\n\nfunction example() {\n  console.log('Hello World!');\n}";
    } else {
      this.textArea.placeholder = "İçeriğinizi yazın...";
    }
  }

  onClose() {
    if (this.onSubmit) {
      this.onSubmit(this.result);
    }
  }
}

const result = await new Promise((resolve) => {
  const modal = new ContentModal(app, resolve);
  modal.open();
});

if (!result || !result.content) return;

const lines = result.content.split('\n');
let formattedContent;

// Dil seçimine göre içeriği formatla
if (result.language) {
  // Kod bloğu olarak formatla
  formattedContent = `> \`\`\`${result.language}\n${lines.map(line => `> ${line}`).join('\n')}\n> \`\`\``;
} else {
  // Normal metin olarak formatla
  formattedContent = lines.map(line => `> ${line}`).join('\n');
}

const calloutBlock = `> [!${type}]${result.state} ${title}\n${formattedContent}`;

tR += calloutBlock;
-%>