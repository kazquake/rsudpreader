# RSUDPReader

RSUDPReader is a repository designed to run RSUDP readers in Docker containers for multiple stations. It outputs STA/LTA (Short-Term Average over Long-Term Average) sensor readings from port 5000 UDP to other ports. The main goal of this repository is to run STA/LTA algorithms from all stations on a single server and then transmit the data via UDP to other services, such as [Presenter](https://github.com/kazquake/presenter).

## Getting Started

These instructions will help you set up and run the RSUDPReader on your local machine for development and testing purposes.

### Prerequisites

- Docker
- Docker Compose

### Installation

1. Clone the repository:
    ```sh
    git clone https://github.com/kazquake/rsudpreader.git
    cd rsudpreader
    ```

2. Ensure Docker and Docker Compose are installed and running on your machine.

## Configuration

The repository uses Docker Compose to manage multiple RSUDP reader instances. Configuration files for each station should be placed in the `configs` directory.

Each RSUDP instance uses a JSON configuration file. For example:
- `rsudp1_settings.json`
- `rsudp2_settings.json`
- `rsudp3_settings.json`

These configuration files should be mapped to the container's expected configuration path as shown in the Docker Compose file.

## Adding Stations

Developers can add their own stations by editing the `docker-compose.yml` file and adding configuration files to the `configs` directory.

### Example

To add a new station:

1. Create a new configuration file in the `configs` directory (e.g., `rsudp4_settings.json`).

2. Add a new service in `docker-compose.yml`:
    ```yaml
    rsudp4:
      image: rishatsultanov/rsudp:v1.0.0
      container_name: rsudp4
      volumes:
        - ./configs/rsudp4_settings.json:/root/.config/rsudp/rsudp_settings.json
      ports:
        - "5004:5000/udp"
        - "8884:8888/udp"
    ```

## Usage

To start the RSUDP readers, run:
```sh
docker-compose up -d
