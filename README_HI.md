# 8K AI Video Enhancer Pro — Android APK starter

यह आपके upload किए गए Real-ESRGAN Colab notebook से आगे बढ़ाया गया स्वतंत्र starter project है। यह Topaz Video AI का clone नहीं है और न ही proprietary Topaz models शामिल करता है।

## क्या जोड़ा गया
- Original notebook के तीन model options रखे: RealESRGAN_x4plus, RealESRGAN_x4plus_anime_6B, realesr-animevideov3.
- Official Real-ESRGAN video script के supported `realesr-general-x4v3` model को denoise strength option के साथ जोड़ा।
- 1080p, 2K, 4K और 8K UHD (7680×4320 landscape / 4320×7680 portrait) output targets।
- GPU tile size option, optional GFPGAN face restoration, FFmpeg sharpening, audio stream mapping।
- Output में crop नहीं; target canvas के भीतर fit + padding।
- Android WebView wrapper और GitHub Actions cloud APK build workflow।

## महत्वपूर्ण सीमाएँ
- 8K output का अर्थ native 8K detail नहीं है। AI अनुमानित texture/detail बनाता है; परिणाम source video और model पर निर्भर हैं।
- Real-ESRGAN x4 models super-resolution करते हैं; यह full Topaz feature set (stabilization, frame interpolation/slow motion, HDR conversion, motion deblur, proprietary models) नहीं है।
- 8K वीडियो बहुत भारी है। पर्याप्त GPU VRAM, RAM, disk, upload/download bandwidth और समय चाहिए। कम VRAM पर tile 128/192/256 करें।
- यह source starter है, signed production APK नहीं। यहां APK compile या GPU processing test नहीं किया गया।
- Colab public share URL temporary है। Colab disconnect होने पर APK में processing नहीं चलेगी। Production APK के लिए permanent HTTPS GPU backend deploy करना अनिवार्य है।

## Android phone-only build plan
1. ZIP को Android फोन में download और extract करें।
2. GitHub पर नया repository बनाएं और extracted project की सभी files upload करें। `.github/workflows/build-apk.yml` सहित hidden folders भी upload हों।
3. पहले GPU backend deploy करें। Colab notebook केवल temporary test के लिए है। Production में GPU VM / GPU hosting पर `backend/app.py` चलाएं, HTTPS domain लें और authentication, upload limits, queue, cleanup तथा privacy controls जोड़ें।
4. GitHub repository → Settings → Secrets and variables → Actions → New repository secret:
   - Name: `APP_URL`
   - Value: आपका deployed HTTPS Gradio app URL
5. GitHub → Actions → Build Android APK → Run workflow।
6. Workflow पूरा होने पर Artifacts में `8k-ai-enhancer-pro-debug-apk` डाउनलोड करें। ZIP extract करके `app-debug.apk` install करें।
7. Release distribution के लिए अलग release keystore से signed release APK बनाना होगा; debug APK public distribution के लिए नहीं है।

## Colab test
1. `8K_AI_Video_Enhancer_Pro_Colab.ipynb` को Colab में खोलें।
2. Runtime → Change runtime type → GPU चुनें।
3. सेल एक-एक करके चलाएं।
4. Upload step में ZIP से `backend/app.py` चुनें।
5. आखिरी सेल चलाएं और temporary Gradio link पर छोटा sample test करें।

## Original notebook से संबंध
आपके मूल notebook में FHD/2K/4K और 2x/3x/4x options थे। मूल output logic में center-crop तथा FFmpeg में duplicate video codec arguments थे। इस starter में aspect-ratio-preserving pad workflow और एक स्पष्ट H.264 output encode रखा गया है।
