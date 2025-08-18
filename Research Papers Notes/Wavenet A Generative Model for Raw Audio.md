Link to paper : https://arxiv.org/pdf/1609.03499.pdf
Link to Blogpost : https://deepmind.google/discover/blog/wavenet-a-generative-model-for-raw-audio/
## TL;DR

WaveNet is a 2016 DeepMind paper that introduced an autoregressive **convolutional** model which generates raw audio waveforms sample-by-sample. It uses **causal + dilated convolutions**, **gated activations**, **residual & skip connections**, and outputs a categorical distribution over µ-law-quantized samples — producing highly natural-sounding speech and convincing music; later work focused on making it fast enough for production. ([Google DeepMind](https://deepmind.google/discover/blog/wavenet-a-generative-model-for-raw-audio/?utm_source=chatgpt.com "WaveNet: A generative model for raw audio - Google DeepMind"))

## Problem statement

- Can we model _raw audio waveforms_ directly (e.g., 16kHz+ sampling) with deep nets, rather than relying on vocoders / hand-crafted features?
    
- How to capture extremely long temporal dependencies in audio without recurrent nets that are slow to train?
    
- Can a single model handle multiple speakers / conditioning signals (text, speaker id, tags)?
    

## Key ideas 

- **Autoregressive factorization** of waveform: $$p(x)=∏tp(xt∣x<t)p(x)=\prod_t p(x_t \mid x_{<t})$$. Generation is sequential but training can be parallel.
    
- **Causal convolutions**: ensure predictions don’t depend on future samples.
    
- **Dilated convolutions**: exponentially growing dilation (1,2,4,…,512, repeat) to get very large receptive fields with relatively few layers.
    
- **Gated activation units** + **residual & skip connections** (inspired by PixelCNN) for stability and effective deep models.
    
- **µ-law companding + 8-bit quantization** (256 levels) → softmax over discrete values, which simplifies output modeling.
    
- **Conditioning**: global (speaker id embeddings) and local (linguistic features for TTS) to guide generation.
    

## Methods — architecture & training

- **Model form**: stack of 1-D causal convolutions (no pooling), outputting at each timestep a categorical distribution over quantized sample values. Training maximizes log-likelihood across timesteps.
    
- **Dilated causal convs**: dilations doubled each layer to exponentially grow receptive field; repetition of dilation stacks increases capacity and receptive field size efficiently.
    
- **Gated activation**: $$z=tanh⁡(Wf∗x+VfTh)⊙σ(Wg∗x+VgTh)\mathrm{z}=\tanh(W_f * x + V_f^T h)\odot\sigma(W_g * x + V_g^T h) $$ (where * is convolution, hh is conditioning). This helps modelling complex interactions.
    
- **Residual & skip connections**: 1×1 convs used to pass features forward and accumulate skip contributions to the final softmax head.
    
- **Quantization**: µ-law transform then 256-level quantize, enabling a tractable softmax output rather than continuous density models.
    

## Experiments & results 

- **Free-form multi-speaker speech (VCTK)**: trained on 44 hours / 109 speakers; WaveNet could produce realistic-sounding speechlike samples and could represent many speakers in a single model via speaker conditioning. Limitations: short coherence beyond the receptive field (~300 ms).
    
- **Text-to-Speech (TTS)**: single-speaker English (24.6h) and Mandarin (34.8h) datasets; WaveNet conditioned on linguistic features produced speech that **human listeners rated significantly more natural** than state-of-the-art parametric and concatenative systems (paired comparisons and MOS tests). In short: subjective naturalness → large improvement.
    
- **Music generation (MagnaTagATune)**: generated short musical fragments that often sounded realistic (quality varied; tags/conditioning helped).
    
- **Speech recognition (TIMIT)**: adapted as a discriminative model (added pooling/frames + frame classifier) and achieved **~18.8% phoneme error rate (PER)** on TIMIT — strong for raw-audio models of the time.
    

## Strengths & limitations (practical)

Strengths

- Produces _very_ high fidelity, natural-sounding audio; simple end-to-end raw waveform modeling; flexible conditioning (speaker, text, tags).
    

Limitations

- **Slow at inference**: autoregressive, sample-by-sample generation is computationally heavy (original WaveNet was far from real-time on CPUs/GPUs).
    
- **Receptive field limits**: though dilations help, very long-term coherence (e.g., multi-second structure in music or discourse) is constrained by finite receptive field.

    
