# Use an official Python runtime as a parent image
FROM python:3.10-slim

WORKDIR /app

ENV HF_HOME=/app/cache
ENV MPLCONFIGDIR /app/matplotlib_cache

# Install necessary dependencies
RUN apt-get update && apt-get install -y \
    build-essential \
    cmake \
    protobuf-compiler \
    git \
    wget \
    libtcmalloc-minimal4 \
    google-perftools \
    libgl1-mesa-glx

# Install xformers module
RUN pip install --upgrade pip

RUN pip install torch torchvision torchaudio --extra-index-url https://download.pytorch.org/whl/cu117 
RUN pip install xformers
# Create a non-root user to run the application
RUN useradd -m -s /bin/bash webui
RUN chown -R webui:webui /app

# Switch to the non-root user
USER webui

# Clone the repository
RUN git clone https://github.com/AUTOMATIC1111/stable-diffusion-webui /app

# Install application dependencies
RUN pip install -r /app/requirements.txt && \
    chmod +x /app/webui.sh

# Expose the port
EXPOSE  7860

# Set the command to run the application
CMD ["./webui.sh","--server-name=0.0.0.0","--skip-torch-cuda-test","--no-half","--use-cpu","all"] 


# "--skip-torch-cuda-test", "--upcast-sampling", "--no-half-vae"]

