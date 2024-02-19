대용량 파일을 분할하여 네트워크를 통해 전송하는 것은 일반적인 관행이며, 파일을 분할하는 것이 전체로 전송하는 것보다 더 효율적일 수 있다는 직관적인 생각이 들지 않더라도 그렇게 하는 데에는 몇 가지 설득력 있는 이유가 있습니다. 개념을 더 쉽게 이해할 수 있도록 비유를 통해 그 이유를 살펴보겠습니다.

### 비유를 통한 기술적 분석

좁은 출입구를 통해 큰 가구를 옮기려고 한다고 상상해 보세요. 가구가 한 조각으로 들어가지 않으므로 가구를 작고 다루기 쉬운 부분으로 분해합니다. 각 부품을 출입구를 통해 쉽게 옮긴 다음 반대편에서 재조립할 수 있습니다. 이는 데이터 전송에서 대용량 파일을 처리하는 방식과 유사합니다.

#### 1. **오류 처리**

**비유:** 한 번에 큰 접시 더미를 옮기다가 비틀거리면 접시를 모두 떨어뜨려 깨뜨릴 위험이 있습니다. 하지만 한 번에 몇 개의 접시를 옮기면 미끄러져도 몇 개의 접시만 떨어뜨려 손실을 최소화할 수 있습니다.

\*\*기술적 세부 사항: 대용량 파일을 하나의 단위로 전송할 때 전송 오류가 발생하면 전체 파일을 다시 보내야 하므로 상당한 대역폭과 시간이 소모될 수 있습니다. 파일을 청킹하면 오류가 발생한 세그먼트만 재전송할 수 있어 오류 복구가 더 효율적입니다.

#### 2. **네트워크 혼잡 및 사용률** 2.

**비유: **혼잡한 고속도로를 생각해 보세요. 트럭을 한꺼번에 보내려고 하면 도로가 막혀 교통 체증을 일으킬 수 있습니다. 그러나 소규모 그룹으로 나누어 배차하면 교통 흐름이 더 원활해지고 모든 차량이 일정한 속도를 유지할 수 있습니다.

**기술적 세부 사항:** 대용량 파일을 한 번에 전송하면 사용 가능한 대역폭을 독점하여 네트워크 정체를 일으킬 수 있습니다. 파일을 청크로 나누면 데이터를 네트워크를 통해 더 고르게 전송할 수 있으므로 다른 데이터 트래픽이 더 원활하게 흐르고 사용 가능한 대역폭을 더 잘 활용할 수 있습니다.

#### 3. **병렬 처리**

**비유:** 큰 퍼즐을 맞출 때는 한 사람이 전체 작업을 하는 것보다 여러 사람이 서로 다른 섹션을 동시에 작업하는 것이 더 빠른 경우가 많습니다.

**기술적 세부 사항:** 청킹을 사용하면 파일의 일부를 여러 연결 또는 다른 경로를 통해 병렬로 처리하거나 전송할 수 있으므로 네트워크 용량이 더 효율적으로 사용되므로 전체 전송 시간이 빨라질 수 있습니다.

### 느려지나요?

파일을 분리했다가 다시 재조립하면 오버헤드가 늘어날 것 같지만, 오류 처리, 네트워크 활용도 향상, 병렬 전송의 가능성은 특히 불안정하거나 혼잡한 네트워크에서 비용보다 더 큰 이점을 가져다주는 경우가 많습니다.

이를 더 잘 이해하기 위해 궁금한 점이 있거나 전제 조건이 있는 특정 분야에 대해 자세히 알아볼 수 있습니다. 예를 들어 TCP/IP 프로토콜, 네트워크 대역폭 또는 오류 수정 메커니즘과 같은 개념에 대해 잘 알고 계신가요? 이러한 개념은 청킹이 효과적인 이유를 이해하는 데 기초가 됩니다.

```python
from concurrent.futures import ThreadPoolExecutor, as_completed
import requests
import os

def file_chunker(file_path, chunk_size=1024*1024):
    """
    Generator function that reads a large file in chunks.

    :param file_path: Path to the large file.
    :param chunk_size: Size of each chunk in bytes.
    :yield: Chunk of the file.
    """
    with open(file_path, 'rb') as file:
        chunk = file.read(chunk_size)
        while chunk:
            yield chunk
            chunk = file.read(chunk_size)

def send_chunk(chunk, chunk_id, url="http://example.com/upload"):
    """
    Function to send a single chunk to the server.

    :param chunk: The file chunk to send.
    :param chunk_id: Identifier for the chunk being sent.
    :param url: URL of the server endpoint to which the chunk is sent.
    :return: Response from the server.
    """
    files = {'file': (f'chunk_{chunk_id}.bin', chunk)}
    response = requests.post(url, files=files)
    return response.status_code, response.text

def upload_file_in_chunks(file_path, chunk_size=1024*1024, parallel_uploads=5):
    """
    Function to upload a file in chunks in parallel.

    :param file_path: Path to the file to be uploaded.
    :param chunk_size: Size of each chunk in bytes.
    :param parallel_uploads: Number of parallel uploads.
    """
    # Initialize ThreadPoolExecutor with the desired number of workers
    with ThreadPoolExecutor(max_workers=parallel_uploads) as executor:
        future_to_chunk = {executor.submit(send_chunk, chunk, chunk_id): chunk_id for chunk_id, chunk in enumerate(file_chunker(file_path, chunk_size))}

        for future in as_completed(future_to_chunk):
            chunk_id = future_to_chunk[future]
            try:
                status_code, response = future.result()
                print(f"Chunk {chunk_id} uploaded with status code {status_code}. Response: {response}")
            except Exception as exc:
                print(f"Chunk {chunk_id} upload failed with exception: {exc}")

# Note: Function calls are commented out to prevent execution within the PCI.
# To use the function, uncomment the following line and replace 'path_to_large_file' with the actual file path.
# upload_file_in_chunks('path_to_large_file')
```

```python
from fastapi import FastAPI, File, UploadFile, HTTPException
from fastapi.responses import JSONResponse
import os
from pathlib import Path
from tempfile import NamedTemporaryFile
import shutil

app = FastAPI()

# Temporary storage for file chunks
temp_storage_path = Path("./temp_storage")
temp_storage_path.mkdir(exist_ok=True)

# Final storage for reassembled files
final_storage_path = Path("./final_storage")
final_storage_path.mkdir(exist_ok=True)

@app.post("/upload_chunk/{file_id}/{chunk_id}")
async def upload_chunk(file_id: str, chunk_id: int, file: UploadFile = File(...)):
    temp_file_path = temp_storage_path / f"{file_id}_{chunk_id}"
    with temp_file_path.open("wb") as buffer:
        shutil.copyfileobj(file.file, buffer)
    return {"file_id": file_id, "chunk_id": chunk_id, "detail": "Chunk uploaded successfully."}

@app.post("/assemble_file/{file_id}")
async def assemble_file(file_id: str):
    chunks = sorted(temp_storage_path.glob(f"{file_id}_*"), key=lambda path: int(path.stem.split("_")[-1]))
    if not chunks:
        raise HTTPException(status_code=404, detail="No chunks found for the given file ID.")

    final_file_path = final_storage_path / f"{file_id}"
    with final_file_path.open("wb") as final_file:
        for chunk_path in chunks:
            with chunk_path.open("rb") as chunk_file:
                shutil.copyfileobj(chunk_file, final_file)
            # Optionally, delete chunk after assembling
            chunk_path.unlink()

    return {"file_id": file_id, "detail": "File assembled successfully."}
```
