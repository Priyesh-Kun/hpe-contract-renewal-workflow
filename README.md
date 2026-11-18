# HPE contract renewal workflow

Download the [workflow transfer ZIP](https://github.com/Priyesh-Kun/hpe-contract-renewal-workflow/releases/download/v1.0.0/customer-contract-renewal-hpe.zip) and [SHA256SUMS](./SHA256SUMS). This bundle preserves source version 2 of Customer Contract Expiry & Renewal Brief, including all 10 steps and its native authoring content.

Copy the ZIP and SHA256SUMS to the HPE accessnode. Verify and extract into a fresh directory:

```sh
sha256sum -c SHA256SUMS
mkdir customer-contract-renewal
unzip customer-contract-renewal-hpe.zip -d customer-contract-renewal
cd customer-contract-renewal
python3 import-workflow.py --check
```

The default check is offline and makes no network requests. Keep the pinned package unchanged before importing. Its example customer ID is `REPLACE_WITH_CUSTOMER_ST_ID`; replace this with the actual customer ID in the workflow UI when running the imported workflow.

On the HPE accessnode, port-forward the API:

```sh
kubectl --context user1-context -n development port-forward svc/dostack-api 18080:8080
```

In another interactive terminal, from the extracted directory:

```sh
python3 import-workflow.py --apply --endpoint http://127.0.0.1:18080
```

Enter the HPE admin email and password at the prompts. The importer checks the active organization is `dostack-hpe`, creates the native workflow through the API, and saves it. It does not execute the workflow. Each `--apply` creates a new workflow. If a request fails, inspect the printed target IDs and HPE UI before retrying; avoid blind reruns.

Argo CD manages application and infrastructure deployment. This importer does not change deployments. HPE needs compatible native authoring support, its own IBSV and RADAR SQL connections, tabular query support, standard and premium model routing, search for step9, and docgen. No credentials or connection configuration are included. No external file or template assets are needed.

The exact HPE API and worker source versions passed offline compilation and execution-plan validation for all 10 tasks. This does not establish live connection configuration or successful HPE execution. Further evidence and transfer details are in the ZIP's README.txt.
