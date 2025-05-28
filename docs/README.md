# Setup

# uv 가 설치 안되어 있으면 먼저 설치한다.

```
pip install uv

cd docs
uv ven .venv  # 가상환경 자동 생성
.venv\Scripts\activate
uv pip install --group docs
```

# 파워셀에서 실행

```
$env:PYTHONUTF8 = "1"    # 환경변수로 Python에 UTF-8 사용을 강제하기
$env:PYTHONPATH = "."
mkdocs serve
```

# CMD일 경우 코드 페이지를 UTF-8로 바꾸기

```
chcp 65001
set PYTHONUTF8=1
mkdocs serve
```

### 브라우저에서 열기: http://localhost:8000

단계 명령어 예시
uv 설치 pip install uv
의존성 설치 (docs/에서) uv pip install --group docs
문서 실행 mkdocs serve
배포 mkdocs gh-deploy

# 기동에 오류가 없으면 깃에 배포한다.

```
(.venv) PS D:\google-ai\langgraph\docs> git remote -v
origin  https://github.com/cbw999/langgraph.git (fetch)
origin  https://github.com/cbw999/langgraph.git (push)
```

- 위와 같이 주소가 제대로 나오는지 확인

# docs/ 안에서 실행(깃배포)

```
.venv\Scripts\activate
$env:PYTHONUTF8 = "1"
$env:PYTHONPATH = "."
mkdocs gh-deploy
```

# 배포가능한 site 폴더가 생성되면서 문서가 만들어 진다. site 폴더를 git gh-pages 브랜치에 자동 배포한다.

https://cbw999.github.io/langgraph

# git 배포시 인증서 오류가 나면 기존 인증서 삭제후 다시 하면 오류 없이 배포된다.

# GitHub Credential Manager 초기화 (현재 로그인 계정 변경)

- 파워셀에서 인증삭제 명령어

```
@"
protocol=https
host=github.com
"@ | git credential-manager erase
```

To setup requirements for building docs you can run:

```bash
uv sync --group test
```

## Serving documentation locally

To run the documentation server locally you can run:

```bash
make serve-docs
```

This will start the documentation server on [http://127.0.0.1:8000/langgraph/](http://127.0.0.1:8000/langgraph/).

## Execute notebooks

If you would like to automatically execute all of the notebooks, to mimic the "Run notebooks" GHA, you can run:

```bash
python _scripts/prepare_notebooks_for_ci.py
./_scripts/execute_notebooks.sh
```

**Note**: if you want to run the notebooks without `%pip install` cells, you can run:

```bash
python _scripts/prepare_notebooks_for_ci.py --comment-install-cells
./_scripts/execute_notebooks.sh
```

`prepare_notebooks_for_ci.py` script will add VCR cassette context manager for each cell in the notebook, so that:

- when the notebook is run for the first time, cells with network requests will be recorded to a VCR cassette file
- when the notebook is run subsequently, the cells with network requests will be replayed from the cassettes

## Adding new notebooks

If you are adding a notebook with API requests, it's **recommended** to record network requests so that they can be subsequently replayed. If this is not done, the notebook runner will make API requests every time the notebook is run, which can be costly and slow.

To record network requests, please make sure to first run `prepare_notebooks_for_ci.py` script.

Then, run

```bash
jupyter execute <path_to_notebook>
```

Once the notebook is executed, you should see the new VCR cassettes recorded in `cassettes` directory and discard the updated notebook.

## Updating existing notebooks

If you are updating an existing notebook, please make sure to remove any existing cassettes for the notebook in `cassettes` directory (each cassette is prefixed with the notebook name), and then run the steps from the "Adding new notebooks" section above.

To delete cassettes for a notebook, you can run:

```bash
rm cassettes/<notebook_name>*
```
