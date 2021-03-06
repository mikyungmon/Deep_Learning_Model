# Improved Residual Networks for Image and Video Recognition # 

### ๐ Abstract ### 

- ResNet์ ๋ค์ํ ์์์์ ๋๋ฆฌ ์ฑํ๋๊ณ  ์ฌ์ฉ๋๋ ๊ฐ๋ ฅํ CNN ์ํคํ์ฒ์ด๋ค.

- ํด๋น ๋ผ๋ฌธ์ ResNet์ ๊ฐ์ ๋ ๋ฒ์ ์ ์ ์ํ๋ค.

- ์ ์ํ๋ ๊ฐ์ ์ฌํญ์ ResNet์ ์ธ ๊ฐ์ง ์ค์ ๊ตฌ์ฑ ์์์ธ ๋คํธ์ํฌ layer์ ํตํ flow of information, residual building block, projection shortcut์ ๋ชจ๋ ๋ค๋ฃฌ๋ค.

- ResNet์ ๋นํด ์ ํ๋์ ํ์ต ์๋ ด์์์ ๊ฐ์ ์ ๋ณด์ฌ์ค ์ ์๋ค.

- ์ ์ํ ๋ฐฉ์์ ์ฌ์ฉํ๋ค๋ฉด ๋งค์ฐ ๊น์ ๋คํธ์ํฌ๋ฅผ ํ๋ จํ  ์ ์์ง๋ง baseline์ ์ฌ๊ฐํ ์ต์ ํ ๋ฌธ์ ๋ฅผ ๋ณด์ฌ์ค๋ค. 

  ๐ *baseline์ด๋ ? ๋จธ์ ๋ฌ๋ ๋ชจ๋ธ์ด ์๋ฏธ์๊ธฐ ์ํด ๋์ด์ผ ํ๋ ์ต์ํ์ ์ฑ๋ฅ. ์ด๋ฏธ์ง ๋ถ๋ฅ(Image Classification)์์๋ ์ผ๋ฐ์ ์ผ๋ก ์ฌ๋์ ์ฑ๋ฅ (Human-Level Performance: HLP)์ ๋ฒ ์ด์ค๋ผ์ธ ์งํ๋ก ์ก์. ๋จธ์ ๋ฌ๋ ๋ชจ๋ธ์ด ์ฌ๋๋ณด๋ค ์ด๋ฏธ์ง ๋ถ๋ฅ๋ฅผ ์ํ๋ค๋ฉด ์๋ฏธ์๋ ๋ชจ๋ธ์ด ๊ตฌ์ถ ๋๋ค๊ณ  ์ฃผ์ฅํ  ์ ์์ ๊ฒ์*

- ImageNet ๋ฐ์ดํฐ ์์์ 404์ธต์ ์ฌ์ธต CNN์ ์ฑ๊ณต์ ์ผ๋ก ํ๋ จํ๊ณ  CIFAR-10 ๋ฐ CIFAR-100์์ 3002์ธต ๋คํธ์ํฌ๋ฅผ ์ฑ๊ณต์ ์ผ๋ก ํ๋ จํ์ง๋ง baseline์ ๊ทธ๋ฌํ ๊ทนํ ๊น์ด์์ ์๋ ดํ  ์ ์๋ค.

- ํด๋น ๋ผ๋ฌธ์ 6๊ฐ์ ๋ฐ์ดํฐ ์์ ๋ํ ์ธ ๊ฐ์ง ์์์ ๋ํ ๊ฒฐ๊ณผ๋ฅผ ๋ณด๊ณ ํ๋ค.

### ๐ 1. Introduction ### 

- ๋คํธ์ํฌ์ ๊น์ด๋ ์๋ง์ ์์์์ ์ธ์์ ์ธ ๊ฒฐ๊ณผ๋ฅผ ๊ฐ์ ธ์ค๋ ๊ฐ๋ ฅํ represetation์ ์ป๋ ๋ฐ ํต์ฌ ์์ ์ค ํ๋๋ก ๊ฐ์กฐ๋์๋ค.

- ์ง๋ ๋ช ๋ ๋์ CNN์ ๊น์ด๋ ์ง์์ ์ผ๋ก ์ฆ๊ฐํด์๋ค. ๊ทธ๋ฌ๋ ๊น์ด๊ฐ ์ฆ๊ฐํจ์ ๋ฐ๋ผ ์ต์ ํ, ํ์ต์ ์ด๋ ค์๋ ํจ๊ป ์ปค์ง๋ค.

- ๋ฐ๋ผ์ ๋ ๋ง์ layer๋ฅผ ์ถ๊ฐํ๋ค๊ณ  ํด์ ๋ ๋์ ๊ฒฐ๊ณผ๊ฐ ๋ณด์ฅ๋๋ ๊ฒ์ ์๋๋ค.

- ResNet์ ๋งค์ฐ ๊น์ CNN ํ์ต ๋ฌธ์ ๋ฅผ residual ํ์ต ์ธก๋ฉด์์ ํด๊ฒฐ์ฑ์ ์ ์ํ๋ค. ์ค์ ๋ก ResNet์ ์ฌ์ธต CNN์ ํ์ตํ๋๋ฐ ๋งค์ฐ ๊ฐ๋ ฅํ๋ฉฐ ๋ณต์กํ ์์์ backbone/foundation์ด ๋๋ค.

- ResNet์ ํต์ฌ ์์ด๋์ด๋ building block์ ๋ํ identity mapping์ shortcut/skip connection์ ์ฌ์ฉํ์ฌ ์ฉ์ดํ๊ฒ ํ๋ ๊ฒ์ด๋ค.

- ์ค์ ๋ก ์ตํฐ๋ง์ด์ ๊ฐ identity mapping์ ํ๋๊ฑด ์ฝ์ง์๊ณ  ์ด๊ฒ์ degradation ๋ฌธ์ (์ ํ ๋ฌธ์ )๋ผ๊ณ  ํ๋ค. 

- ResNet์ ์์ด๋์ด๋ ์ฑ๋ฅ ์ ํ ๋ฌธ์ ๋ฅผ ํด๊ฒฐํ์ฌ ํจ์ฌ ๋ ๊น์ ๋คํธ์ํฌ๋ฅผ ํจ์จ์ ์ผ๋ก ํ์ตํ  ์ ์๋ค๋ ๊ฒ์ด๋ค.

- ๊ทธ๋ฌ๋ degradation ๋ฌธ์ ๊ฐ ์์ ํ ํด๊ฒฐ๋์ง ์์๊ณ  ํด๋น ๋ผ๋ฌธ์ ์คํ์์ ์ฆ๋ช๋์๋ค. ์๋ฅผ ๋ค์ด ImageNet๋ฐ์ดํฐ ์์์ ๊น์ด๋ฅผ 152๊ฐ์์ 200๊ฐ๋ก ๋๋ฆฌ๋ฉด train error๋ฅผ ํฌํจํ ํจ์ฌ ๋ ๋์ ๊ฒฐ๊ณผ๊ฐ ๋ํ๋ ์ฌ๊ฐํ ์ต์ ํ ๋ฌธ์ ๊ฐ ์์์ ์์ฌํ๋ค.
    
    โก layer์๊ฐ ์ฆ๊ฐํ  ๋ ResNet์ ์ฌ์ ํ ๋คํธ์ํฌ๋ฅผ ํตํ ์ ๋ณด์ propagation์ ๋ฐฉํดํ๋ค๋ ๊ฒ์ ๋ณด์ฌ์ค๋ค.
    
- ๋ฐ๋ผ์ ํด๋น ๋ผ๋ฌธ์์๋ ์ ๋ณด propagation์ ์ฉ์ดํ๊ฒ ํ๋ ๊ฐ์ ๋ ์ํคํ์ฒ๋ฅผ ์ ์ํ๋ค. ๋คํธ์ํฌ ๋จ๊ณ๋ก ๋ถ๋ฆฌํ๊ณ  ๊ฐ ๋จ๊ณ ๋ด์ ์์น์ ๋ฐ๋ผ ๋ค๋ฅธ building block์ ์ ์ฉํ๋ค.
  
    โก ๋งค์ฐ ์ฌ์ธต์ ์ธ ๋คํธ์ํฌ ํ์ต์ด ๊ฐ๋ฅํ๋ฉฐ ๊น์ด๊ฐ ์ฆ๊ฐํด๋ ์ต์ ํ์ ์ด๋ ค์์ ๋ณด์ด์ง ์์๋ค.

- ResNet์์๋ building block์ ์ฐจ์์ด next builidng block์ ์ฐจ์๊ณผ ๋ง์ง ์์ ๋, projection shortcut์ ์ฌ์ฉํ๋ค. 

- ์ด ์์์ degradation๋ฌธ์ ๋ฅผ ์ํด ํ์์ ์ด์ง ์๋ค๋ ๊ฒฐ๋ก ์ ๋ด๋ ธ๋ค. ๊ทธ๋ฌ๋ projection shortcut์ ์ฃผ์ ์ ๋ณด ์ ํ ๊ฒฝ๋ก์์ ๋ฐ๊ฒฌ๋์ด ์ ํธ๋ฅผ ์ฝ๊ฒ ๋์์ํค๊ฑฐ๋ ์ ๋ณด ์์ค์ ์ผ์ผํฌ ์ ์๊ธฐ ๋๋ฌธ์ ๋คํธ์ํฌ ์ํคํ์ฒ์์ ์ค์ํ ์ญํ ์ ํ  ์ ์๋ค.

- ๋ฐ๋ผ์ ์ฌ๊ธฐ์์๋ ๋งค๊ฐ๋ณ์๊ฐ ์๊ณ  ์ฑ๋ฅ์ด ํฌ๊ฒ ํฅ์๋๋ ๊ฐ์ ๋ shortcut projection์ ์๊ฐํ๋ค.

- ์๋ ResNet์์๋ ๊น์ด๊ฐ ์๋นํ ์ฆ๊ฐํ  ๋ ๋งค๊ฐ๋ณ์์ ์์ ๊ณ์ฐ ๋น์ฉ์ ์ ์ดํ๊ธฐ ์ํด bottleneck building block์ ์ฌ์ฉํ์๋ค. ๊ทธ๋ฌ๋ ์ด building block ๊ตฌ์กฐ์์ spatial ํํฐ ํ์ต์ ๋ด๋นํ๋ ์ ์ผํ ์ปจ๋ณผ๋ฃจ์์ ์ต์์ input/output ์ฑ๋์ ์์ ํ๋ค.

- ํด๋น ๋ผ๋ฌธ์ spatial ์ปจ๋ณผ๋ฃจ์์ ์ด์ ์ ๋ณ๊ฒฝํ๋ building block์ ์ ์ํ๋ค. ์๋ ResNet๋ณด๋ค building block์ 4๋ฐฐ ๋ ๋ง์ spatial ์ฑ๋์ ํฌํจํ๋ ๋์์ ๋งค๊ฐ๋ณ์์ ์์ ๊ณ์ฐ ๋น์ฉ์ ์ ์ดํ๋ค.

๐ก ์์ฝํ๋ฉด ํด๋น ๋ผ๋ฌธ์ contribution์ ๋ค์๊ณผ ๊ฐ๋ค.

  1) ๋จ๊ณ ๊ธฐ๋ฐ์ residual learning์ ์ํ ๋คํธ์ํฌ ์ํคํ์ฒ๋ฅผ ์๊ฐํ๋ค. ์ ์๋ ์ ๊ทผ ๋ฐฉ์์ ์ ๋ณด๊ฐ ๋คํธ์ํฌ layer๋ฅผ ํตํด ์ ํ๋๋ ๋ ๋์ ๊ฒฝ๋ก๋ฅผ ์ ๊ณตํ์ฌ ํ์ต ํ๋ก์ธ์ค๋ฅผ ์ฉ์ดํ๊ฒ ํ๋ค.

  2) ์ ๋ณด ์์ค์ ์ค์ด๊ณ  ๋ ๋์ ๊ฒฐ๊ณผ๋ฅผ ์ ๊ณตํ๋ ๊ฐ์ ๋ projection shortcut์ ์ ์ํ๋ค.

  3) ๋ณด๋ค ๊ฐ๋ ฅํ spatial ํจํด์ ํ์ตํ๊ธฐ ์ํด spatial ์ฑ๋์ ์๋นํ ์ฆ๊ฐ์ํค๋ building block์ ์ ์ํ๋ค.

  4) ์ ์๋ ์ ๊ทผ ๋ฐฉ์์ baseline์ ๋ํด ์ผ๊ด๋ ๊ฐ์ ์ ์ ๊ณตํ๋ค.


### ๐ 2. Improved residual network ### 

   #### 2.1 Improved information flow through the network #### 
   
   - ResNet์ ๋ง์ ResBlock์ ์์์ ๊ตฌ์ฑ๋๋ค.
   
   ![image](https://user-images.githubusercontent.com/66320010/144838766-89c645d9-b17f-40ac-a016-606577961563.png)
   
   - ์ ๊ทธ๋ฆผ์์ ์๋ ResBlock์์ ํฐ ํ์ ํ์ดํ๋ ์ ๋ณด๊ฐ ์ ํ๋๋ ๊ฐ์ฅ ์ง์ ์ ์ธ ๊ฒฝ๋ก๋ฅผ ๋ํ๋ธ๋ค.

   - ๊ทธ๋ฌ๋ ์ฃผ ์ ํ ๊ฒฝ๋ก(main propagation path)์ ReLU ํ์ฑํ ํจ์๊ฐ ์๋ค. ์ด ReLU๋ ์์ ์ ํธ๋ฅผ ์์ ํํจ์ผ๋ก์จ ์ ๋ณด propagation์ ๋ถ์ ์ ์ธ ์ํฅ์ ๋ฏธ์น  ์ ์๋ค. 
   
   - ๊ทธ๋์ ๋ง์ง๋ง BN์ธต๊ณผ ReLU๋ฅผ ์ฒ์์ผ๋ก ์ด๋ํ์ฌ pre-activation(์ฌ์  ํ์ฑํ)๋ผ๊ณ  ๋ถ๋ฆฌ๋ ์ฌ์ค๊ณ๋ ResBlock์ ์ ์ํ๋ค(๊ทธ๋ฆผ (b)).

   - ํ์ง๋ง ์ด ๋๊ฐ์ง ๋ฐฉ๋ฒ ๋ชจ๋ ์ต์ ์ด ์๋๋ฉฐ ๋ค๋ฅธ ๋ฌธ์ ๊ฐ ์๋ค. ์ฒซ์งธ, ๋ค ๋จ๊ณ ๋ชจ๋์ ๊ฑธ์ณ ์ ์ฒด ์ ํธ์ ์ ๊ทํ(BN)๊ฐ ์์ผ๋ฏ๋ก ๋ ๋ง์ ๋ธ๋ก์ ์ถ๊ฐํจ์ ๋ฐ๋ผ ์ ์ฒด ์ ํธ๊ฐ ๋ "๋น์ ๊ทํ"๋์ด ํ์ต์ ์ด๋ ค์์ด ๋ฐ์ํ๋ค. ์ด ๋ฌธ์ ๋ ์๋์ ResNet๊ณผ pre-act๋ชจ๋์ ์กด์ฌํ๋ค.

   - ๋์งธ, 4๊ฐ์ prejection shortcut์ด ์๋ค๋ ์ ์ ์ ์ํ์. ์ด๋ก ์ ์ผ๋ก ๋คํธ์ํฌ๋ ๋๋ถ๋ถ์ block์ ๋ํ identity matching์ ํ์ตํ  ์ ์๋ค. 0์ ์ถ๋ ฅํ  ์ ์์ผ๋ฏ๋ก ๋ํ๊ธฐ ์ฐ์ฐ์ ์ ์ฉํ  ๋ identity matching์ ์ฝ๊ฒ ์์ฑํ  ์ ์๋ค.
   
   - ์ด ๊ฒฝ์ฐ ๋ชจ๋  4๊ฐ์ ์ฃผ์ ๋จ๊ณ์ ๋ํ pre-activation, ResNet์ 4๊ฐ์ ์ฐ์์ ์ธ 1 x 1 conv๋ก ๋๋์ง๋ง ๊ทธ ์ฌ์ด์ ๋น์ ํ์ฑ์ด ์์ด ํ์ต ๋ฅ๋ ฅ์ด ์ ํ๋๋ค.

   - ํด๋น ๋ผ๋ฌธ์ ๊ฐ ๋ฉ์ธ ์คํ์ด์ง ์ด์ ์ ์ ํธ๋ฅผ ์์ ํํ๊ณ (๊ฐ ๋ฉ์ธ ์คํ์ด์ง ์ดํ์ ์ ์ฒด ์ ํธ์ BN์ ์ฌ์ฉํจ) ์ ์ด๋ ํ๋์ ๋น์ ํ์ฑ (์ ์ฒด ์ ํธ์๋ ์ ์ฉ๋จ) ์ด ์์์ ๋ณด์ฅํ๊ธฐ ๋๋ฌธ์ ์ด ๋๊ฐ์ง ๋ฌธ์ ๋ ํด๊ฒฐํ๋ค.

   - ์ ์ํ ResBlock์ ์ ๊ทธ๋ฆผ(c)์ ๊ฐ๋ค. ๊ตฌ์ฒด์ ์ธ ์๋ก์ ๋ค์ ํ์ ๋์์๋ 50 ๊น์ด์ ResNet(ResNet-50)์ ์ทจํ  ์ ์๊ณ  ์ด๊ฑฐ๋ ์ด๋ ๊น์ด๋ก๋ ํ์ฅ๋  ์ ์๋ค.
    
   ![image](https://user-images.githubusercontent.com/66320010/144841621-2ddef929-7bf8-4cd7-b815-14b9b13eb5e4.png)
   
   - ResNet-50์ ๊ฐ๋ฅํ ๋จ๊ณ ๋ถ๋ฆฌ๋ ์ถ๋ ฅ spatial ํฌ๊ธฐ์ ์ถ๋ ฅ ์ฑ๋ ์์ ๋ฐ๋ผ ๊ฒฐ์ ๋๋ค.

   - ์ถ๋ ฅ spatial ํฌ๊ธฐ๋ ์ถ๋ ฅ ์ฑ๋ ์๊ฐ ๋ณ๊ฒฝ๋  ๋ ๋ค๋ฅธ ๋จ๊ณ์ ์์์ ํ์ํ๋ค. ResNet-50์ ๊ฒฝ์ฐ 4๊ฐ์ ์ฃผ์ ๋จ๊ณ์ ์์ ๋ฐ ์ข๋ฃ ๋จ๊ณ๋ฅผ ์ป๋๋ค. 4๊ฐ์ ์ฃผ์ ๋จ๊ณ ๊ฐ๊ฐ์ ๋ค์์ ResBlock์ ํฌํจํ  ์ ์๋ค.

   - ResNet-50์ ๊ฒฝ์ฐ 1๋จ๊ณ์ 3๊ฐ์ ResBlock, 2๋จ๊ณ์ 4๊ฐ, 3๋จ๊ณ์ 6๊ฐ, 4๋จ๊ณ์ 3๊ฐ๊ฐ ์๋ค.
  
   - ๊ฐ ์ฃผ์ ๋จ๊ณ๋ ์ธ ๋ถ๋ถ์ผ๋ก ๋๋๋ค. ํ๋๋ Start ResBlock, ์ฌ๋ฌ ๊ฐ์ Middle ResBlock, ํ๋๋ End ResBlock์ด๋ค.

   - ์ฌ๊ธฐ์ ResNet์ ์ ์๋ ๋จ๊ณ ์ํคํ์ฒ๋ก ๋ถํ ํ ๊ฒฐ๊ณผ๋ฅผ ResStage ๋คํธ์ํฌ๋ผ๊ณ  ๋ถ๋ฅธ๋ค.
   
   - ์ ์๋ ResStage์ ๊ฒฝ์ฐ Start ResBlock์ ์ํด ์ ํธ๊ฐ ์ด๋ฏธ ์ ๊ทํ๋์๊ธฐ ๋๋ฌธ์ ๊ฐ ์ฃผ์ ๋จ๊ณ์์ ์ฒซ ๋ฒ์งธ middle ResBlock์์ ์ฒซ ๋ฒ์งธ BN์ ์ ๊ฑฐํ๋ค. 
   
   - ์ ์๋ ResStage๋ ์๋์ ๋ฐฑ์๋์ ๋ํ ์ ๋ณด๊ฐ ์ ํ๋๋ ์ฃผ์ ๊ฒฝ๋ก์ ๊ณ ์ ๋ ์์ ReLU๋ฅผ ํฌํจํ๋ค. ์๋ฅผ ๋ค์ด ์ฃผ ์ ํ ๊ฒฝ๋ก์ ์๋ ReLU์ ์๋ ๋คํธ์ํฌ ๊น์ด์ ์ ๋น๋กํ๋ค.
   
   - ResStage์ ์๋ ๋์ ์ฃผ์ ๋จ๊ณ์ ๊ฒฝ์ฐ ๊ธฐ๋ณธ ์ ๋ณด ์ ํ ๊ฒฝ๋ก์ ReLU๊ฐ 4๊ฐ๋ฟ์ด๋ฉฐ ๊น์ด ๋ณ๊ฒฝ์ ์ํฅ์ ๋ฐ์ง ์๋๋ค. ์ด๋ฅผ ํตํด ์ ๋ณด๊ฐ ์ฌ๋ฌ ๊ณ์ธต์ ํต๊ณผํ  ๋ ๋คํธ์ํฌ๊ฐ ์ ํธ๋ฅผ ๋ฐฉํดํ๋ ๊ฒ์ ๋ฐฉ์งํ  ์ ์๋ค.

   - ๊ฐ ๋จ๊ณ์ End Resblock์ BN๊ณผ ReLU๋ก ์๋ฃ๋๋ฉฐ, ์ด๋ ๋ค์ ๋จ๊ณ๋ฅผ ์ํ ์ค๋น, ์์ ํ ๋ฐ ์๋ก์ด ๋จ๊ณ๋ก ์ง์ํ๊ธฐ ์ํ ์ ํธ๋ฅผ ์ค๋นํ๋ ๊ฒ์ผ๋ก ๋ณผ ์ ์๋ค.

   - Start ResBlock์๋ ์ ํธ๋ฅผ ์ ๊ทํํ๊ณ  projection shortcut(์ ๊ทํ๋ ์ ํธ๋ ์ ๊ณต)๋ฅผ ์ฌ์ฉํ์ฌ ์์๋ณ ์ถ๊ฐ๋ฅผ ์ํด ์ค๋นํ๋ ๋ง์ง๋ง conv ๋ค์ BN ์ธต์ด ์๋ค.

   - ์ ์ํ ์ ๊ทผ ๋ฐฉ์์ ์ ๋ณด๊ฐ ๋คํธ์ํฌ๋ฅผ ํตํด ์ ํ๋๋ ๋ ๋์ ๊ฒฝ๋ก๋ฅผ ์ ๊ณตํจ์ผ๋ก์จ ํ์ต์ ์ฉ์ดํ๊ฒ ํ๋ค. 
  
   - ResStage๋ ์ต์ ํ๋ฅผ ์ฉ์ดํ๊ฒ ํ์ฌ ๋งค์ฐ ๊น์ ๋คํธ์ํฌ๋ฅผ ์ฝ๊ฒ ํ๋ จํ  ์ ์๋๋ก ํ๋ค. 
  
   - ๋คํธ์ํฌ๋ ํ์ต ๊ณผ์ ์์ ์ด๋ค ResBlock์ ์ฌ์ฉํ๊ณ  ์ด๋ค ResBlock์ ๋ฒ๋ฆด์ง(๊ฐ์ค์น๋ฅผ ์ฝ๊ฒ 0์ผ๋ก ์ค์ ํ์ฌ) ์ฝ๊ฒ ๋์ ์ผ๋ก ์ ํํ  ์ ์๋ค. ๋จ๊ณ๋ณ ํ์ต์ ์ํ ์ ์์ ํจ์จ์ ์ธ ์ ๋ณด ํ๋ฆ์ ์ํด ์ค๊ณ๋์์ง๋ง ์ ํธ๋ฅผ ์ ์ดํ  ์ ์๋๋ก ์ค๊ณ๋์๋ค.

   #### 2.2 Improved projection shortcut #### 
   
   - ์๋ ResNet ์ํคํ์ฒ์์ x์ ์ฐจ์์ด F์ ์ฐจ์ ์ถ๋ ฅ๊ณผ ์ผ์นํ์ง ์์ ๋ ์์๋ณ ์ถ๊ฐ๋ฅผ ๊ฐ๋ฅํ๊ฒ ํ๊ธฐ ์ํด identity shortcut๋์  projection shortcut์ด x์ ์ ์ฉ๋๋ค.

   - ์๋ ResNet์์ ์ฌ์ฉ๋ ๊ธฐ๋ณธ projection shortcut์ ๋ค์ ๊ทธ๋ฆผ์์ (a)์ ๊ฐ๋ค.

   ![image](https://user-images.githubusercontent.com/66320010/144979821-7629871f-64c4-4d92-83bc-a9b97061d6a0.png)

   - ์๋ projection shortcut์ 1 x 1 ์ปค๋์ ์ปจ๋ณผ๋ฃจ์์ ์ฌ์ฉํ์ฌ x์ ์ฑ๋์ F์ ์ถ๋ ฅ ์ฑ๋ ์๋ก projectionํ๋ค(1 x 1 ์ปจ๋ณผ๋ฃจ์์ stride 2๋ก ์งํํ๊ณ  F์ ์ถ๋ ฅ๊ณผ ์์๋ณ ๋ง์ ์ ์ BN์ด ์ ์ฉ๋๋ค).
   - ๊ทธ๋ฌ๋ฏ๋ก ์ฑ๋๊ณผ spatial matching์ 1 x 1 ์ปจ๋ณผ๋ฃจ์์ ์ํด ์ํ๋๋ค.
   
   - ์ด๋ spatial size๋ฅผ 2๋ก ์ค์ผ ๋ 1 x 1 ์ปจ๋ณผ๋ฃจ์(with stride 2)์ด featrue maps activations์ 75%๋ฅผ ๊ฑด๋๋ฐ๊ธฐ ๋๋ฌธ์ ์ ๋ณด์ ์๋นํ ์์ค์ ์ด๋ํ๋ค. ๋ํ 1 x 1 ์ปจ๋ณผ๋ฃจ์์์ ๊ณ ๋ ค๋๋ feature map activation์ 25%๋ฅผ ์ ํํ๊ธฐ ์ํ ์๋ฏธ ์๋ ๊ธฐ์ค์ด ์๋ค.
   
   - ๊ทธ ํ ๊ฒฐ๊ณผ๋ ResBlock์ output์ ๋ํด์ง๋ค. ๋ฐ๋ผ์ projection shortcut์ noisy output์ ๋ค์ ResBlock์ ์๋์ ์ผ๋ก ์ ๋ณด์ ์ ๋ฐ์ ๊ธฐ์ฌํ๋ค.
   
   - ์ด๊ฒ์ noise์ ์ ๋ณด ์์ค์ ์ผ๊ธฐํ๊ณ  ๋คํธ์ํฌ๋ฅผ ํตํ ์ ๋ณด์ ์ฃผ์ ํ๋ฆ์ ๋ถ์ ์ ์ผ๋ก ๊ต๋์ํฌ ์ ์๋ค.

   - ์ ์๋ projection shortcut์ ์์ ๊ทธ๋ฆผ์์ (b)์ ๊ฐ๋ค. ์ฑ๋ projection์์ ์ฑ๋ projection์ ๋ถ๋ฆฌํ๋ค.
   - ๊ทธ๋ฆฌ๊ณ  spatial projection์ ์ํด stride 2๋ก 3 x 3 max pooling์ ์ํํ๋ค.
   - ๊ทธ๋ฐ๋ค์ ์ฑ๋ projection์ ์ํด stride 1๋ก 1 x 1 ์ปจ๋ณผ๋ฃจ์์ ์ ์ฉํ ๋ค์ BN์ ์ ์ฉํ๋ค.
   - max pooling๊ณผ ํจ๊ป, ํ์ฑํ๋ฅผ ์ํด 1 x 1 ์ปจ๋ณผ๋ฃจ์๋ํด ๊ณ ๋ ค๋์ด์ผํ๋ criterion์ ๋์ํ๋ค.
   - ๋ํ spatial projection์ feature map์ ๋ชจ๋  ์ ๋ณด๋ฅผ ๊ณ ๋ คํ๊ณ  ๋ค์ ๋จ๊ณ์์ ๊ณ ๋ คํ  ๊ฐ์ฅ ๋์ ํ์ฑํ๋ฅผ ๊ฐ์ง ์์๋ฅผ ์ ํํ๋ค.
   - max pooling ์ ์ปค๋์ ResBlock์ middle conv์ ์ปค๋๊ณผ ์ผ์นํ๋ฏ๋ก ๋์ผํ spatial window์์ ๊ณ์ฐ๋ ์์๊ฐ์ ์ถ๊ฐ์ ์ธ ์์๊ฐ ์ํ๋๋ค.
   - ์ ์ํ projection shortcut์ ์ ๋ณด ์์ค์ ์ค์ด๊ณ  ์คํ์์ ์ฑ๋ฅ์์ ์ด์ ์ ๋ณด์ฌ์ค๋ค.
   - ์ ๋ณด ์์ค๊ณผ ์ ํธ ๋์๋ฅผ ์ค์ด๊ธฐ ์ํ ์ฒซ๋ฒ์งธ motivation ์ด์ธ์๋ ์ ์ํ projection shortcut์ด ๋ค๋ฅธ ๋ ๊ฐ์ง ์ด์ ๊ฐ ์๋ค.
   - ๋ ๋ฒ์งธ๋ก, ๊ฐ ์ฃผ์ ๋จ๊ฒ์ start ResBlock์ max pooling์ ๊ฐ๋ ๊ฒ์ ๋คํธ์ํฌ ๋ณํ ๋ถ๋ณ์ฑ์ ๊ฐ์ ํ๊ณ  ๊ถ๊ทน์ ์ผ๋ก ์ ๋ฐ์ ์ธ ์ธ์ ์ฑ๋ฅ์ ํฅ์ ์ํจ๋ค.
   - ์ธ ๋ฒ์งธ๋ก๋ ๋ค์ด ์ํ๋ง์ ์ํํ๋ ๊ฐ ๋จ๊ณ์ start ResBlock์ 3 x 3 ์ปจ๋ณผ๋ฃจ์์ ์ํด ์ํ๋๋ ์ํํธ ๋ค์ด ์ํ๋ง ๊ฐ์ ์กฐํฉ์ผ๋ก ๋ณผ ์ ์๋ค.
   - ํ๋ ๋ค์ด์ํ๋ง์ ๋ถ๋ฅ(๊ฐ์ฅ ๋์ ํ์ฑํ๊ฐ ์๋ ์์)์ ์ ๋ฆฌํ๊ณ  ์ํํธ ๋ค์ด ์ํ๋ง์ ๋ชจ๋  spatial ๋งฅ๋ฝ์ ์์ง ์๋๋ฐ ๊ธฐ์ฌํ๋ค(๋ฐ๋ผ์ ์์๊ฐ์ ์ ํ์ด ๋ ์ํํ  ๋ ๋ ๋์ localization์ ๋๋๋ค)
   - ์ ์๋ projection shortcut์ ๋ชจ๋ธ์ ์ถ๊ฐ ๋งค๊ฐ๋ณ์๋ฅผ ์ถ๊ฐํ์ง ์๋๋ค. ResNet์ ์ผ๋ฐ์ ์ผ๋ก ๊ฐ ๋จ๊ณ์ ์์(๋จ๊ณ์ ResBlock ์์)์ 4๊ฐ์ projection shortcut๋ง ํ์ํฉ๋๋ค. 
   - ๋ฐ๋ผ์ ์ ์๋ projection shortcut์ ๊ฒฝ์ฐ ์ผ๋ฐ์ ์ผ๋ก ๊ณ์ฐ ๋น์ฉ์ด ์ ๋ ดํ ์ต๋ ํ๋ง ๋ ์ด์ด 3๊ฐ๋ง ์ถ๊ฐ๋ก ํฌํจํ๋ฉด ๋๊ธฐ ๋๋ฌธ์ ResNet์ ๊ณ์ฐ ๋น์ฉ ์ฆ๊ฐ๋ ๋ฌด์ํ  ์ ์๋ค.
   - ์ ์๋ projection shortcut์ ํจ๊ป ์ด์  ์น์์ ๋จ๊ณ ์์ด๋์ด๋ฅผ ์ฌ์ฉํ  ๋ ๊ฐ์ ๋ ์์ฌ ๋คํธ์ํฌ(iResNet)๋ฅผ ์ฐธ์กฐํ๋ค.
  
   #### 2.3 Grouped building block #### 
   
   - bottleneck building block์ ๋คํธ์ํฌ ๊น์ด๋ฅผ ์ฆ๊ฐ์ํฌ ๋ ํฉ๋ฆฌ์ ์ธ ๊ณ์ฐ ๋น์ฉ์ ์ ์งํ๊ธฐ ์ํ ์ค์ง์ ์ธ ๊ณ ๋ ค์ฌํญ์ผ๋ก ๋์๋์๋ค.
   
   - ๋จผ์  ์ฑ๋ ์๋ฅผ ์ค์ด๊ธฐ ์ํ 1 x 1 ์ปจ๋ณผ๋ฃจ์, ๊ฐ์ฅ ์์ ์์ ์๋ ฅ/์ถ๋ ฅ์์ ์๋ํ๋ 3 x 3 ์ปจ๋ณผ๋ฃจ์ bottleneck, ๋ง์ง๋ง์ผ๋ก ์๋์ ์ฑ๋ ์๋ก ๋ค์ ๋๋ฆฌ๋ 1 x 1 ์ปจ๋ณผ๋ฃจ์์ ํฌํจํ๋ค.
   
   - ์ด ์ค๊ณ์ ์ด์ ๋ ์ ์ ์์ ์ฑ๋์์ 3 ร 3 ์ปจ๋ณผ๋ฃจ์์ ์คํํ์ฌ ๊ณ์ฐ ๋น์ฉ๊ณผ ๋งค๊ฐ ๋ณ์์ ์๋ฅผ ์ ์ดํ๊ธฐ ์ํจ์ด๋ค.
   - ๊ทธ๋ฌ๋ 3ร3 conv๋ ๊ณต๊ฐ ํจํด์ ํ์ตํ  ์ ์๋ ์ ์ผํ ๊ตฌ์ฑ ์์์ด๋ฏ๋ก ๋งค์ฐ ์ค์ํ์ง๋ง bottleneck ์ค๊ณ์์๋ ๋ ์ ์ ์์ ์๋ ฅ/์ถ๋ ฅ ์ฑ๋์ ์์ ํ๋ค.
   - ํด๋น ๋ผ๋ฌธ์ 3ร3 ์ปจ๋ณผ๋ฃจ์์์ ๊ฐ์ฅ ๋ง์ ์์ ์๋ ฅ/์ถ๋ ฅ ์ฑ๋์ ํฌํจํ๋ ๊ฐ์ ๋ building block์ ์ ์ํ๋ค.
   - ์ ์๋ building block์ ์ค๊ณ์์ ๊ทธ๋ฃนํ๋ ์ปจ๋ณผ๋ฃจ์์ ์ฌ์ฉํ๋ฉฐ ์ด๋ฅผ ResGroup Block์ด๋ผ๊ณ  ํ๋ค.
   - ๊ทธ๋ฃนํ๋ ์ปจ๋ณผ๋ฃจ์์ ๊ณ์ฐ ๋น์ฉ๊ณผ ๋ฉ๋ชจ๋ฆฌ๋ก ์ธํ ์ ํ์ ๊ทน๋ณตํ๊ธฐ ์ํด ๋ ๊ฐ์ GPU์ ๋ชจ๋ธ์ ๋ฐฐํฌํ๋ ์๋ฃจ์์ผ๋ก ์ฌ์ฉ๋์๋ค.
   - ๊ทธ๋ฃนํ๋ ์ปจ๋ณผ๋ฃจ์์ ์ฃผ์ ์์ด๋์ด๋ ์๋ ฅ ์ฑ๋์ ์ฌ๋ฌ ๊ทธ๋ฃน์ผ๋ก ๋ถํ ํ๊ณ  ๊ฐ ๊ทธ๋ฃน์ ๋ํด ๋๋ฆฝ์ ์ผ๋ก ์ปจ๋ณผ๋ฃจ์ ์์์ ์ํํ๋ ๊ฒ์ด๋ค.
   - ์ด๋ฌํ ๋ฐฉ์์ผ๋ก ๋งค๊ฐ๋ณ์(param)์ ๋ถ๋์์์  ์ฐ์ฐ(FLOP)์ ์๋ฅผ ๊ทธ๋ฃน ์์ ๋์ผํ ๋น์จ๋ก ์ค์ผ ์ ์๋ค.

   ![image](https://user-images.githubusercontent.com/66320010/144991624-67567e4f-a59a-489c-a753-752d9bd63e1f.png)
   
   - ์ฌ๊ธฐ์ chin๊ณผ chout์ ์๋ ฅ ๋ฐ ์ถ๋ ฅ ์ฑ๋์ ์, k1๊ณผ k2๋ ์ปจ๋ณผ๋ฃจ์์ kernel์ ํฌ๊ธฐ๋ฅผ ๋ํ๋ด๊ณ  w์ h๋ ์ฑ๋์ ๋๋น์ ๋์ด, G๋ ์ฑ๋์ด ๋ถํ ๋ ๊ทธ๋ฃน์ ์๋ฅผ ๋ํ๋ธ๋ค.
   
   - ๊ธฐ์กด ResNet-50๊ณผ ์ ์ฌํ ๊ณ์ฐ ๋น์ฉ๊ณผ ๋งค๊ฐ๋ณ์ ์๋ฅผ ๊ฐ๋ ์ ์๋ ๋คํธ์ํฌ ์ํคํ์ฒ๋ ์์์ ์ธ๊ธํ ํ์ ๋์ ์๋ค.
   
   - ๋งค๊ฐ๋ณ์์ ์์ ๊ณ์ฐ ๋น์ฉ์ ์ ์ดํ๊ธฐ ์ํด ๊ทธ๋ฃนํ๋ ์ปจ๋ณผ๋ฃจ์์ 3 ร 3 spatial ์ปค๋๊ณผ ํจ๊ป ์ฌ์ฉ๋๋ค.
 
   - ๋ ๊ฐ์ง ์ํคํ์ฒ๋ฅผ ์ ์
      1. ResGroupFix-50์ ๊ฐ ๋จ๊ณ์ ๋ํ ๊ทธ๋ฃน ์๊ฐ ๊ณ ์ ๋ ๊ฒฝ์ฐ๋ฅผ ๋ํ๋. ์ด ์ต์์ 50๊ฐ ๋ ์ด์ด์ ๋ํ baseline๋ณด๋ค ์ ์ฌํ ์์ FLOP์ 8.57% ์ ์ ํ๋ผ๋ฏธํฐ๋ฅผ ์์ฑํจ.
      
      2. ResGroup-50์ ๋ชจ๋  ์คํ์ด์ง๊ฐ ๊ทธ๋ฃน๋น ๋์ผํ ์์ ์ฑ๋์ ๊ฐ๋ ๋ฐฉ์์ผ๋ก ์ฑ๋์ ๊ทธ๋ฃน ์๋ฅผ ์กฐ์ ํ๋ ๊ฒฝ์ฐ๋ฅผ ๋ํ๋. ResGroup-50์ ์๋ ResNet-50 ๋ฐ ๋ ๋ง์ FLOP์ ์ ์ฌํ ์์ ๋งค๊ฐ ๋ณ์๋ฅผ ๊ฐ์ง๊ณ  ์์.
   
   ![image](https://user-images.githubusercontent.com/66320010/144993808-31f0ac9f-48e6-4d01-bf79-a3aeb0762cbd.png)
   
   - ๋คํธ์ํฌ์ ๋ง์ง๋ง ์์ฌ ๋น๋ ๋ธ๋ก์ ๋ํ ์์์ ์ธ ๋น๊ต๋ ์ ๊ทธ๋ฆผ์ ์ฐธ์กฐ
   
   - ์ด ์ ๊ทผ๋ฒ์ผ๋ก, 3ร3์ ๊ฐ์ฅ ๋ง์ ์์ ์ฑ๋์ ๊ฐ์ง๊ณ  ์๊ณ  spatial ํจํด์ ํ์ตํ  ์ ์๋ ๋ ๋์ ๋ฅ๋ ฅ์ ๊ฐ์ง๊ณ  ์๋ค.
   
