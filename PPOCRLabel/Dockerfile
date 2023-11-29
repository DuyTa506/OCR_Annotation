FROM python:3.9
ENV DEBIAN_FRONTEND=noninteractive

# Install system dependencies
RUN apt-get update && \
    apt-get upgrade -y && \
    apt-get dist-upgrade -y && \
    apt-get autoremove -y && \
    apt-get install -y qttools5-dev-tools libpulse-dev libqt5multimedia5-plugins libqt5widgets5 libqt5gui5 libqt5dbus5 libqt5network5 libqt5core5a libxcb-xinerama0

# Create user
RUN adduser --quiet --disabled-password qtuser && usermod -a -G audio qtuser

# Set environment variables
ENV LIBGL_ALWAYS_INDIRECT=1
ENV QT_DEBUG_PLUGINS=1
ENV QT_QPA_PLATFORM=xcb
ENV QT_QPA_PLATFORM_PLUGIN_PATH=/opt/Qt/${QT_VERSION}/gcc_64/plugins
ENV QT_PLUGIN_PATH=/opt/Qt/${QT_VERSION}/gcc_64/plugins
ENV DISPLAY=:1

# Copy the application code
COPY . /app

# Set the working directory
WORKDIR /app

# Create and activate virtual environment
RUN python -m venv venv
ENV PATH="/app/venv/bin:$PATH"

# Upgrade pip and install Python dependencies
RUN /app/venv/bin/python -m pip install --upgrade pip

WORKDIR /app/OCR_Annotation/PPOCRLabel/

RUN /app/venv/bin/python -m pip install -r requirements.txt 
#requirements.txt instead of above
RUN /app/venv/bin/python -m pip uninstall -y opencv-python
RUN /app/venv/bin/python -m pip install opencv-python-headless



# Default command
CMD ["python3", "PPOCRLabel.py", "--lang", "vi"]
