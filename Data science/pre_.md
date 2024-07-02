### 1.对于数据集的数理
 1. 首先，对于含有缺失值的列的处理：删除缺失值大于 90% 的列，删除列中记录数据小于 30 的列
### 2. 对于数据集中的某些特征处理   
这里先讲解对于 Smiles、E3 ligase和 Target的处理：        
#### 2.1 Simles: 这里我采用的是从```Smiles``` 字符串中提取具体的分子描述符(分子量、LogP、氢键供体和氢键受体数量)。代码如下：  
    ```Python    
    from rdkit import Chem
    from rdkit.Chem import Descriptors

    def extract_features(smiles):
        mol = Chem.MolFromSmiles(smiles)
        if mol:
            features = {
                'MolWt': Descriptors.MolWt(mol),
                'LogP': Descriptors.MolLogP(mol),
                'NumHDonors': Descriptors.NumHDonors(mol),
                'NumHAcceptors': Descriptors.NumHAcceptors(mol),
            }
        else:
            features = {
                'MolWt': None,
                'LogP': None,
                'NumHDonors': None,
                'NumHAcceptors': None,
        }
        return features
    ```
这里我还在 kaggle 上面看到对于```Smiles```中提取化学描述符对象(如：拓扑极性表面积 (TPSA)、精确分子量、价电子数和杂原子数)。地址是：https://www.kaggle.com/code/vladislavkisin/tutorial-ml-in-chemistry-research-rdkit-mol2vec
#### 2.2 对于 E3 ligase和 Target 的处理
对于它们都进行了标签编码，对于 Target 根据其功能进行了编码。
