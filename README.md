# Kanana
<p align="center">
<br>
    <picture>
        <source media="(prefers-color-scheme: dark)" srcset="assets/logo/kanana-logo-light.png">
        <source media="(prefers-color-scheme: light)" srcset="assets/logo/kanana-logo-dark.png">
        <img alt="Kanana Logo" src="assets/logo/kanana-logo-light.png" width="400" style="margin: 40px auto;">
    </picture>
</br>
<p align="center"> 🤗 <a href="https://huggingface.co/collections/kakaocorp/kanana-nano-21b-67a326cda1c449c8d4172259">Models</a> &nbsp | &nbsp 📕 <a href="https://tech.kakao.com/posts/689">Blog</a> &nbsp | &nbsp 📜 <a href="./paper/KananaV1_Technical_Report.pdf">Technical Report</a>

<br>

<br>

## Introduction

We introduce Kanana, a series of bilingual language models (developed by [Kakao](https://github.com/kakao)) that demonstrate exceeding performance in Korean and competitive performance in English. The computational cost of Kanana is significantly lower than that of state-of-the-art models of similar size. The report details the techniques employed during pre-training to achieve compute-efficient yet competitive models, including high-quality data filtering, staged pre-training, depth up-scaling, and pruning and distillation. Furthermore, the report outlines the methodologies utilized during the post-training of the Kanana models, encompassing supervised fine-tuning and preference optimization, aimed at enhancing their capability for seamless interaction with users. Lastly, the report elaborates on plausible approaches used for language model adaptation to specific scenarios, such as embedding, function calling, and Retrieval Augmented Generation (RAG). The Kanana model series spans from 2.1B to 32.5B parameters with 2.1B models (base, instruct, embedding, function call, and RAG) publicly released to promote research on Korean language models.

> [!Note]
> Neither the pre-training nor the post-training data includes Kakao user data.

<p align="center">
<picture>
    <img src="assets/performance/flops-vs-mmlu.jpg", width="700" style="margin: 40px auto;">
</picture>

<br>

## Table of Contents

- [News](#news)
- [Performance](#performance)
- [Quickstart](#quickstart)
- [License](#license)
- [Citation](#citation)
- [Contributors](#contributors)
- [Contact](#contact)

<br>

## News

- 📜`2025/02/27`: Released [Technical Report](./paper/KananaV1_Technical_Report.pdf) and 🤗[HF model weights](https://huggingface.co/collections/kakaocorp/kanana-nano-21b-67a326cda1c449c8d4172259).
- 📕`2025/01/10`: Published a blog post about the development of `Kanana-Nano` model. ([Kanana-Nano](https://tech.kakao.com/posts/682))
- 📕`2024/11/14`: Published blog posts about the development of `Kanana` models. ([Kanana LLM: Pre-training](https://tech.kakao.com/posts/661), [Kanana LLM: Post-training](https://tech.kakao.com/posts/662))
- ▶️`2024/11/06`: Published a presentation video about the development of the `Kanana` models. ([if(kakaoAI)2024](https://youtu.be/HTBl142x9GI?si=o_we6t9suYK8DfX3))


<br>

## Performance

Below are partial report on the performance of the `Kanana` model series. Please refer to the [Technical Report](./paper/KananaV1_Technical_Report.pdf) for the full results.

### Pre-trained Model Performance

<table>
    <tr>
        <th>Models</th>
        <th>MMLU</th>
        <th>KMMLU</th>
        <th>HAERAE</th>
        <th>HumanEval</th>
        <th>MBPP</th>
        <th>GSM8K</th>
    </tr>
    <tr>
        <th colspan="8" height="30px">27b+ scale</th>
    </tr>
    <tr>
        <td>Kanana-Flag-32.5b</td>
        <td align="center">77.68</td>
        <td align="center">62.10</td>
        <td align="center"><strong>90.47</strong></td>
        <td align="center"><strong>51.22</strong></td>
        <td align="center">63.40</td>
        <td align="center">70.05</td>
    </tr>
    <tr>
        <td>Qwen2.5-32b</td>
        <td align="center"><strong>83.10</strong></td>
        <td align="center"><strong>63.15</strong></td>
        <td align="center">75.16</td>
        <td align="center">50.00</td>
        <td align="center">73.40</td>
        <td align="center"><strong>82.41</strong></td>
    </tr>
    <tr>
        <td>Gemma-2-27b</td>
        <td align="center">75.45</td>
        <td align="center">51.16</td>
        <td align="center">69.11</td>
        <td align="center"><strong>51.22</strong></td>
        <td align="center">64.60</td>
        <td align="center">74.37</td>
    </tr>
    <tr>
        <td>EXAONE-3.5-32b</td>
        <td align="center">72.68</td>
        <td align="center">46.36</td>
        <td align="center">82.22</td>
        <td align="center">-</td>
        <td align="center">-</td>
        <td align="center">-</td>
    </tr>
    <tr>
        <td>Aya-Expanse-32b</td>
        <td align="center">74.52</td>
        <td align="center">49.57</td>
        <td align="center">80.66</td>
        <td align="center">-</td>
        <td align="center">-</td>
        <td align="center">-</td>
    </tr>
    <tr>
        <th colspan="8" height="30px">7b+ scale</th>
    </tr>
    <tr>
        <td>Kanana-Essence-9.8b</td>
        <td align="center">67.61</td>
        <td align="center">50.57</td>
        <td align="center"><strong>84.98</strong></td>
        <td align="center">40.24</td>
        <td align="center">53.60</td>
        <td align="center">63.61</td>
    </tr>
    <tr>
        <td>Llama-3.1-8b</td>
        <td align="center">65.18</td>
        <td align="center">41.02</td>
        <td align="center">61.78</td>
        <td align="center">35.37</td>
        <td align="center">48.60</td>
        <td align="center">50.87</td>
    </tr>
    <tr>
        <td>Qwen2.5-7b</td>
        <td align="center"><strong>74.19</strong></td>
        <td align="center"><strong>51.68</strong></td>
        <td align="center">67.46</td>
        <td align="center"><strong>56.71</strong></td>
        <td align="center"><strong>63.20</strong></td>
        <td align="center"><strong>83.85</strong></td>
    </tr>
    <tr>
        <td>Gemma-2-9b</td>
        <td align="center">70.34</td>
        <td align="center">48.18</td>
        <td align="center">66.18</td>
        <td align="center">37.20</td>
        <td align="center">53.60</td>
        <td align="center">68.16</td>
    </tr>
    <tr>
        <td>EXAONE-3.5-7.8b</td>
        <td align="center">65.36</td>
        <td align="center">45.30</td>
        <td align="center">77.54</td>
        <td align="center">-</td>
        <td align="center">-</td>
        <td align="center">-</td>
    </tr>
    <tr>
        <td>Aya-Expanse-8b</td>
        <td align="center">62.52</td>
        <td align="center">40.11</td>
        <td align="center">71.95</td>
        <td align="center">-</td>
        <td align="center">-</td>
        <td align="center">-</td>
    </tr>
    <tr>
        <th colspan="8" height="30px">2b+ scale</th>
    </tr>
    <tr>
        <td>Kanana-Nano-2.1b</td>
        <td align="center">54.83</td>
        <td align="center">44.80</td>
        <td align="center"><strong>77.09</strong></td>
        <td align="center">31.10</td>
        <td align="center">46.20</td>
        <td align="center">46.32</td>
    </tr>
    <tr>
        <td>Llama-3.2-3b</td>
        <td align="center">56.40</td>
        <td align="center">35.57</td>
        <td align="center">47.66</td>
        <td align="center">25.61</td>
        <td align="center">39.00</td>
        <td align="center">27.37</td>
    </tr>
    <tr>
        <td>Qwen2.5-3b</td>
        <td align="center"><strong>65.57</strong></td>
        <td align="center"><strong>45.28</strong></td>
        <td align="center">61.32</td>
        <td align="center"><strong>37.80</strong></td>
        <td align="center"><strong>55.60</strong></td>
        <td align="center"><strong>69.07</strong></td>
    </tr>
    <tr>
        <td>Gemma-2-2b</td>
        <td align="center">52.89</td>
        <td align="center">30.67</td>
        <td align="center">45.55</td>
        <td align="center">20.12</td>
        <td align="center">28.20</td>
        <td align="center">24.72</td>
    </tr>
    <tr>
        <td>EXAONE-3.5-2.4b</td>
        <td align="center">59.27</td>
        <td align="center">43.58</td>
        <td align="center">69.65</td>
        <td align="center">-</td>
        <td align="center">-</td>
        <td align="center">-</td>
    </tr>
    <tr>
        <th colspan="8" height="30px">70b+ scale</th>
    </tr>
    <tr>
        <td>Llama-3.1-70b</td>
        <td align="center">78.93</td>
        <td align="center">53.00</td>
        <td align="center">76.35</td>
        <td align="center">57.32</td>
        <td align="center">66.60</td>
        <td align="center">81.73</td>
    </tr>
    <tr>
        <td>Qwen2.5-72b</td>
        <td align="center">86.12</td>
        <td align="center">68.57</td>
        <td align="center">80.84</td>
        <td align="center">55.49</td>
        <td align="center">76.40</td>
        <td align="center">92.04</td>
    </tr>
</table>

<br>


### Post-trained Model Performance

#### Instruction-following Benchmarks
<table>
    <tr>
        <th>Models</th>
        <th>MT-Bench</th>
        <th>LogicKor</th>
        <th>KoMT-Bench</th>
        <th>WildBench</th>
        <th>IFEval</th>
    </tr>
    <tr>
        <th colspan="8" height="30px">27b+ scale</th>
    </tr>
    <tr>
        <td>Kanana-Flag-32.5b</td>
        <td align="center">8.356</td>
        <td align="center"><strong>9.524</strong></td>
        <td align="center"><strong>8.058</strong></td>
        <td align="center">54.14</td>
        <td align="center"><strong>0.856</strong></td>
    </tr>
    <tr>
        <td>Qwen2.5-32b</td>
        <td align="center">8.331</td>
        <td align="center">8.988</td>
        <td align="center">7.847</td>
        <td align="center">51.13</td>
        <td align="center">0.822</td>
    </tr>
    <tr>
        <td>Gemma-2-27b</td>
        <td align="center">8.088</td>
        <td align="center">8.869</td>
        <td align="center">7.373</td>
        <td align="center">46.46</td>
        <td align="center">0.817</td>
    </tr>
    <tr>
        <td>EXAONE-3.5-32b</td>
        <td align="center"><strong>8.375</strong></td>
        <td align="center">9.202</td>
        <td align="center">7.907</td>
        <td align="center"><strong>54.30</strong></td>
        <td align="center">0.845</td>
    </tr>
    <tr>
        <td>Aya-Expanse-32b</td>
        <td align="center">7.788</td>
        <td align="center">8.941</td>
        <td align="center">7.626</td>
        <td align="center">48.36</td>
        <td align="center">0.735</td>
    </tr>
    <tr>
        <th colspan="8" height="30px">7b+ scale</th>
    </tr>
    <tr>
        <td>Kanana-Essence-9.8b</td>
        <td align="center">7.769</td>
        <td align="center">8.964</td>
        <td align="center">7.706</td>
        <td align="center">47.27</td>
        <td align="center">0.799</td>
    </tr>
    <tr>
        <td>Llama-3.1-8b</td>
        <td align="center">7.500</td>
        <td align="center">6.512</td>
        <td align="center">5.336</td>
        <td align="center">33.20</td>
        <td align="center">0.772</td>
    </tr>
    <tr>
        <td>Qwen2.5-7b</td>
        <td align="center">7.625</td>
        <td align="center">7.952</td>
        <td align="center">6.808</td>
        <td align="center">41.31</td>
        <td align="center">0.760</td>
    </tr>
    <tr>
        <td>Gemma-2-9b</td>
        <td align="center">7.633</td>
        <td align="center">8.643</td>
        <td align="center">7.029</td>
        <td align="center">40.92</td>
        <td align="center">0.750</td>
    </tr>
    <tr>
        <td>EXAONE-3.5-7.8b</td>
        <td align="center"><strong>8.213</strong></td>
        <td align="center"><strong>9.357</strong></td>
        <td align="center"><strong>8.013</strong></td>
        <td align="center"><strong>50.98</strong></td>
        <td align="center"><strong>0.826</strong></td>
    </tr>
    <tr>
        <td>Aya-Expanse-8b</td>
        <td align="center">7.131</td>
        <td align="center">8.357</td>
        <td align="center">7.006</td>
        <td align="center">38.50</td>
        <td align="center">0.645</td>
    </tr>
    <tr>
        <th colspan="8" height="30px">2b+ scale</th>
    </tr>
    <tr>
        <td>Kanana-Nano-2.1b</td>
        <td align="center">6.400</td>
        <td align="center">7.964</td>
        <td align="center">5.857</td>
        <td align="center">25.41</td>
        <td align="center">0.720</td>
    </tr>
    <tr>
        <td>Llama-3.2-3b</td>
        <td align="center">7.050</td>
        <td align="center">4.452</td>
        <td align="center">3.967</td>
        <td align="center">21.91</td>
        <td align="center">0.767</td>
    </tr>
    <tr>
        <td>Qwen2.5-3b</td>
        <td align="center">6.969</td>
        <td align="center">6.488</td>
        <td align="center">5.274</td>
        <td align="center">25.76</td>
        <td align="center">0.355</td>
    </tr>
    <tr>
        <td>Gemma-2-2b</td>
        <td align="center">7.225</td>
        <td align="center">5.917</td>
        <td align="center">4.835</td>
        <td align="center">28.71</td>
        <td align="center">0.428</td>
    </tr>
    <tr>
        <td>EXAONE-3.5-2.4b</td>
        <td align="center"><strong>7.919</strong></td>
        <td align="center"><strong>8.941</strong></td>
        <td align="center"><strong>7.223</strong></td>
        <td align="center"><strong>41.68</strong></td>
        <td align="center"><strong>0.790</strong></td>
    </tr>
    <tr>
        <th colspan="8" height="30px">70b+ scale</th>
    </tr>
    <tr>
        <td>Llama-3.1-70b</td>
        <td align="center">8.275</td>
        <td align="center">8.250</td>
        <td align="center">6.970</td>
        <td align="center">46.50</td>
        <td align="center">0.875</td>
    </tr>
    <tr>
        <td>Qwen2.5-72b</td>
        <td align="center">8.619</td>
        <td align="center">9.214</td>
        <td align="center">8.281</td>
        <td align="center">55.25</td>
        <td align="center">0.861</td>
    </tr>
</table>

<br>

#### General Benchmarks

<table>
    <tr>
        <th>Models</th>
        <th>MMLU</th>
        <th>KMMLU</th>
        <th>HAE-RAE</th>
        <th>HumanEval+</th>
        <th>MBPP+</th>
        <th>GSM8K</th>
        <th>MATH</th>
    </tr>
    <tr>
        <th colspan="8" height="30px">27b+ scale</th>
    </tr>
    <tr>
        <td>Kanana-Flag-32.5b</td>
        <td align="center">81.08</td>
        <td align="center"><strong>64.19</strong></td>
        <td align="center"><strong>68.18</strong></td>
        <td align="center">77.44</td>
        <td align="center">69.84</td>
        <td align="center">90.83</td>
        <td align="center">57.82</td>
    </tr>
    <tr>
        <td>Qwen2.5-32b</td>
        <td align="center"><strong>84.40</strong></td>
        <td align="center">59.37</td>
        <td align="center">48.30</td>
        <td align="center"><strong>82.32</strong></td>
        <td align="center"><strong>71.96</strong></td>
        <td align="center"><strong>95.30</strong></td>
        <td align="center"><strong>81.90</strong></td>
    </tr>
    <tr>
        <td>Gemma-2-27b</td>
        <td align="center">78.01</td>
        <td align="center">49.98</td>
        <td align="center">46.02</td>
        <td align="center">70.12</td>
        <td align="center">70.90</td>
        <td align="center">91.05</td>
        <td align="center">53.80</td>
    </tr>
    <tr>
        <td>EXAONE-3.5-32b</td>
        <td align="center">78.30</td>
        <td align="center">55.44</td>
        <td align="center">52.27</td>
        <td align="center">78.66</td>
        <td align="center">70.90</td>
        <td align="center">93.56</td>
        <td align="center">76.80</td>
    </tr>
    <tr>
        <td>Aya-Expanse-32b</td>
        <td align="center">74.49</td>
        <td align="center">42.35</td>
        <td align="center">51.14</td>
        <td align="center">64.63</td>
        <td align="center">65.61</td>
        <td align="center">75.06</td>
        <td align="center">42.82</td>
    </tr>
    <tr>
        <th colspan="8" height="30px">7b+ scale</th>
    </tr>
    <tr>
        <td>Kanana-Essence-9.8b</td>
        <td align="center">70.64</td>
        <td align="center">50.76</td>
        <td align="center"><strong>47.16</strong></td>
        <td align="center">72.56</td>
        <td align="center">69.05</td>
        <td align="center">84.91</td>
        <td align="center">42.24</td>
    </tr>
    <tr>
        <td>Llama-3.1-8b</td>
        <td align="center">71.18</td>
        <td align="center">39.24</td>
        <td align="center">40.91</td>
        <td align="center">60.98</td>
        <td align="center">57.67</td>
        <td align="center">82.71</td>
        <td align="center">49.86</td>
    </tr>
    <tr>
        <td>Qwen2.5-7b</td>
        <td align="center"><strong>77.23</strong></td>
        <td align="center">46.87</td>
        <td align="center">37.50</td>
        <td align="center">73.78</td>
        <td align="center"><strong>70.63</strong></td>
        <td align="center"><strong>91.58</strong></td>
        <td align="center"><strong>75.22</strong></td>
    </tr>
    <tr>
        <td>Gemma-2-9b</td>
        <td align="center">73.47</td>
        <td align="center">44.47</td>
        <td align="center">39.77</td>
        <td align="center">59.76</td>
        <td align="center">64.55</td>
        <td align="center">87.72</td>
        <td align="center">48.10</td>
    </tr>
    <tr>
        <td>EXAONE-3.5-7.8b</td>
        <td align="center">72.62</td>
        <td align="center"><strong>52.09</strong></td>
        <td align="center">46.02</td>
        <td align="center"><strong>79.27</strong></td>
        <td align="center">66.67</td>
        <td align="center">89.99</td>
        <td align="center">73.50</td>
    </tr>
    <tr>
        <td>Aya-Expanse-8b</td>
        <td align="center">61.23</td>
        <td align="center">35.78</td>
        <td align="center">39.20</td>
        <td align="center">42.68</td>
        <td align="center">56.88</td>
        <td align="center">78.85</td>
        <td align="center">30.80</td>
    </tr>
    <tr>
        <th colspan="8" height="30px">2b+ scale</th>
    </tr>
    <tr>
        <td>Kanana-Nano-2.1b</td>
        <td align="center">52.48</td>
        <td align="center"><strong>38.51</strong></td>
        <td align="center"><strong>33.52</strong></td>
        <td align="center">63.41</td>
        <td align="center">62.43</td>
        <td align="center">72.32</td>
        <td align="center">29.26</td>
    </tr>
    <tr>
        <td>Llama-3.2-3b</td>
        <td align="center">56.09</td>
        <td align="center">3.07</td>
        <td align="center">17.05</td>
        <td align="center">56.71</td>
        <td align="center">50.26</td>
        <td align="center">66.57</td>
        <td align="center">38.18</td>
    </tr>
    <tr>
        <td>Qwen2.5-3b</td>
        <td align="center"><strong>69.18</strong></td>
        <td align="center">38.33</td>
        <td align="center">32.39</td>
        <td align="center">67.68</td>
        <td align="center"><strong>64.02</strong></td>
        <td align="center"><strong>84.00</strong></td>
        <td align="center"><strong>65.72</strong></td>
    </tr>
    <tr>
        <td>Gemma-2-2b</td>
        <td align="center">57.69</td>
        <td align="center">6.99</td>
        <td align="center">7.95</td>
        <td align="center">35.37</td>
        <td align="center">45.24</td>
        <td align="center">49.81</td>
        <td align="center">21.68</td>
    </tr>
    <tr>
        <td>EXAONE-3.5-2.4b</td>
        <td align="center">63.19</td>
        <td align="center">14.27</td>
        <td align="center">14.20</td>
        <td align="center"><strong>70.73</strong></td>
        <td align="center">59.79</td>
        <td align="center">83.78</td>
        <td align="center">64.04</td>
    </tr>
    <tr>
        <th colspan="8" height="30px">70b+ scale</th>
    </tr>
    <tr>
        <td>Llama-3.1-70b</td>
        <td align="center">83.48</td>
        <td align="center">39.08</td>
        <td align="center">53.41</td>
        <td align="center">75.61</td>
        <td align="center">66.40</td>
        <td align="center">91.66</td>
        <td align="center">63.98</td>
    </tr>
    <tr>
        <td>Qwen2.5-72b</td>
        <td align="center">87.14</td>
        <td align="center">65.78</td>
        <td align="center">60.80</td>
        <td align="center">81.10</td>
        <td align="center">75.66</td>
        <td align="center">95.45</td>
        <td align="center">82.60</td>
    </tr>
</table>

<br>

### Embedding Model Performance
<table>
    <tr>
        <td align="center">Backbone</td>
        <td align="center">Kanana-Nano-2.1b</td>
        <td align="center">Llama-3.2-3b</td>
        <td align="center">Qwen2.5-3b</td>
        <td align="center">Llama-3.2-1b</td>
        <td align="center">Qwen-2.5-1.5b</td>
    </tr>
    <tr>
        <td align="center">English</td>
        <td align="center">51.56</td>
        <td align="center">53.28</td>
        <td align="center"><strong>54.00</strong></td>
        <td align="center">48.77</td>
        <td align="center">50.60</td>
    </tr>
    <tr>
        <td align="center">Korean</td>
        <td align="center"><strong>65.00</strong></td>
        <td align="center">59.43</td>
        <td align="center">62.10</td>
        <td align="center">54.68</td>
        <td align="center">54.60</td>
    </tr>
    <tr>
        <td align="center">Avg.</td>
        <td align="center"><strong>58.28</strong></td>
        <td align="center">56.35</td>
        <td align="center">58.05</td>
        <td align="center">51.73</td>
        <td align="center">52.60</td>
    </tr>
</table>

<br>

## Quickstart

### 🤗 HuggingFace Transformers

- `transformers>=4.45.0` or the latest version is required to run `Kanana` model.
```bash
pip install transformers>=4.45.0
```

#### Example Usage for `kanana-nano-2.1b-base`
```python
import torch
from transformers import AutoModelForCausalLM, AutoTokenizer

model_name = "kakaocorp/kanana-nano-2.1b-base"

model = AutoModelForCausalLM.from_pretrained(
    model_name,
    torch_dtype=torch.bfloat16,
    trust_remote_code=True,
).to("cuda")
tokenizer = AutoTokenizer.from_pretrained(model_name, padding_side="left")
tokenizer.pad_token = tokenizer.eos_token

prompt1 = "이처럼 인간처럼 생각하고 행동하는 AI 모델은 "
prompt2 = "Kakao is a leading company in South Korea, and it is known for "

input_ids = tokenizer(
    [prompt1, prompt2],
    padding=True,
    return_tensors="pt",
)["input_ids"].to("cuda")

_ = model.eval()
with torch.no_grad():
    output = model.generate(
        input_ids,
        max_new_tokens=32,
        do_sample=False,
    )

decoded = tokenizer.batch_decode(output, skip_special_tokens=True)
for text in decoded:
    print(text)

# Output:
# 이처럼 인간처럼 생각하고 행동하는 AI 모델은 2020년대 중반에 등장할 것으로 예상된다. 2020년대 중반에 등장할 것으로 예상되는 AI 모델은 인간
# Kakao is a leading company in South Korea, and it is known for 1) its innovative products and services, 2) its commitment to sustainability, and 3) its focus on customer experience. Kakao has been recognized as
```

#### Example Usage for `kanana-nano-2.1b-instruct`
```python
import torch
from transformers import AutoModelForCausalLM, AutoTokenizer

model_name = "kakaocorp/kanana-nano-2.1b-instruct"

model = AutoModelForCausalLM.from_pretrained(
    model_name,
    torch_dtype=torch.bfloat16,
    trust_remote_code=True,
).to("cuda")
tokenizer = AutoTokenizer.from_pretrained(model_name)

prompt = "Convert these dates to YYYY/MM/DD format:\n12/31/2021\n02-01-22"
messages = [
    {"role": "system", "content": "You are a helpful AI assistant developed by Kakao."},
    {"role": "user", "content": prompt}
]

input_ids = tokenizer.apply_chat_template(
    messages,
    tokenize=True,
    add_generation_prompt=True,
    return_tensors="pt"
).to("cuda")

_ = model.eval()
with torch.no_grad():
    output = model.generate(
        input_ids,
        max_new_tokens=72,
        do_sample=False,
    )

print(tokenizer.decode(output[0], skip_special_tokens=True))

# Output:
# Sure! Here are the given dates converted to the `YYYY/MM/DD` format:

# 1. **12/31/2021**
#    - **YYYY/MM/DD:** 2021/12/31

# 2. **02-01-22**
#    - **YYYY/MM/DD:** 2022/02/01

# So, the converted dates are ...
```

#### Example Usage for `kanana-nano-2.1b-embedding`

> [!Note]
> You need to install `datasets` via `pip install datasets` before using `kanana-nano-2.1b-embedding` model.

```python
import torch.nn.functional as F
from transformers import AutoModel

instruction = "Given a question, retrieve passages that answer the question"
queries = [
    "are judo throws allowed in wrestling?", 
    "how to become a radiology technician in michigan?",
]

passages = [
    "Since you're reading this, you are probably someone from a judo background or someone who is just wondering how judo techniques can be applied under wrestling rules. So without further ado, let's get to the question. Are Judo throws allowed in wrestling? Yes, judo throws are allowed in freestyle and folkstyle wrestling. You only need to be careful to follow the slam rules when executing judo throws. In wrestling, a slam is lifting and returning an opponent to the mat with unnecessary force.",
    "Below are the basic steps to becoming a radiologic technologist in Michigan:Earn a high school diploma. As with most careers in health care, a high school education is the first step to finding entry-level employment. Taking classes in math and science, such as anatomy, biology, chemistry, physiology, and physics, can help prepare students for their college studies and future careers.Earn an associate degree. Entry-level radiologic positions typically require at least an Associate of Applied Science. Before enrolling in one of these degree programs, students should make sure it has been properly accredited by the Joint Review Committee on Education in Radiologic Technology (JRCERT).Get licensed or certified in the state of Michigan.",
]

model = AutoModel.from_pretrained(
    "kakaocorp/kanana-nano-2.1b-embedding",
    trust_remote_code=True,
).to("cuda")

max_length = 512
query_embeddings = model.encode(queries, instruction=instruction, max_length=max_length)
passage_embeddings = model.encode(passages, instruction="", max_length=max_length)

# get the embeddings with DataLoader (spliting the datasets into multiple mini-batches)
# batch_size = 2
# query_embeddings = model._do_encode(queries, batch_size=batch_size, instruction=instruction, max_length=max_length)
# passage_embeddings = model._do_encode(passages, batch_size=batch_size, instruction="", max_length=max_length)

query_embeddings = F.normalize(query_embeddings, p=2, dim=1)
passage_embeddings = F.normalize(passage_embeddings, p=2, dim=1)


scores = (query_embeddings @ passage_embeddings.T) * 100
print(scores.tolist())

# Output:
# [[84.36527252197266, 31.752296447753906], [35.940425872802734, 81.82719421386719]]
```

<br>

### vLLM
#### Example Usage for `kanana-nano-2.1b-base`
```bash
vllm serve kakaocorp/kanana-nano-2.1b-base

curl http://localhost:8000/v1/completions -H "Content-Type: application/json" -d '{
    "model": "kakaocorp/kanana-nano-2.1b-base",
    "prompt": "Kakao is a leading company in South Korea, and it is known for ",
    "max_tokens": 32,
    "top_k": 1
}'

# Output:
'''
...
"choices":[{"index":0,"text":"1) its innovative products and services, 2) its commitment to sustainability, and 3) its focus on customer experience. Kakao has been recognized as", ...
...
'''
```

#### Example Usage for `kanana-nano-2.1b-instruct`
```bash
vllm serve kakaocorp/kanana-nano-2.1b-instruct

curl http://localhost:8000/v1/chat/completions -H "Content-Type: application/json" -d '{
  "model": "kakaocorp/kanana-nano-2.1b-instruct",
  "messages": [
    {"role": "system", "content": "You are a helpful AI assistant developed by Kakao."},
    {"role": "user", "content": "Convert these dates to YYYY/MM/DD format:\n12/31/2021\n02-01-22"}
  ],
  "top_k": 1,
  "max_tokens": 72
}'

# Output:
'''
...
"choices":[{"index":0,"message":{"role":"assistant","content":"Sure! Here are the given dates converted to the `YYYY/MM/DD` format:\n\n1. **12/31/2021**\n   - **YYYY/MM/DD:** 2021/12/31\n\n2. **02-01-22**\n   - **YYYY/MM/DD:** 2022/02/01\n\nSo, the converted dates are", ...
...
'''
```


<br>

## License

The `Kanana` models are licensed under [CC-BY-NC-4.0](./LICENSE).
 
<br>
 
## Citation
 
- (TBD)

<br>

## Contributors
- Pre-training: Yunju Bak, Doohae Jung, Boseop Kim, Nayeon Kim, Hojin Lee, Jaesun Park,  Minho Ryu
- Post-training: Jiyeon Ham, Seungjae Jung, Hyunho Kim, Hyunwoong Ko, Changmin Lee, Daniel Wontae Nam, Kyoung-Woon On
- Adaptation: Seulye Baeg, Junrae Cho, Taegyeong Eo, Sunghee Jung, Jieun Kang, EungGyun Kim, Eunhwa Kim, Byeongil Ko, Daniel Lee, Donghun Lee, Minchul Lee, Miok Lee, Shinbok Lee, Minho Ryu, Gaeun Seo

<br>

## Contact
- Kanana LLM Team Technical Support: kanana-llm@kakaocorp.com
- Business & Partnership Contact: alpha.k@kakaocorp.com