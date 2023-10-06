# DEPRECATED REPO. MOVED TO -> https://github.com/steelswing/assimp4j-52

# Assimp for java
This library works without LWJGL 3!

# Examples
### Static mesh loader
```java
  public static final Set<AiPostProcessSteps> ASSIMP_POST = new HashSet<AiPostProcessSteps>() {
      private static final long serialVersionUID = 1L;

      {
          add(AiPostProcessSteps.TRIANGULATE);
          add(AiPostProcessSteps.GEN_SMOOTH_NORMALS);
          add(AiPostProcessSteps.FLIP_UVS);
          add(AiPostProcessSteps.CALC_TANGENT_SPACE);
          add(AiPostProcessSteps.JOIN_IDENTICAL_VERTICES);
          add(AiPostProcessSteps.OPTIMIZE_MESHES);
          add(AiPostProcessSteps.FIX_INFACING_NORMALS);
      }
  };
  
  public static Mesh load(File filePath, int meshId) throws IOException {
      AiScene scene = IHMCJassimp.importFile(filePath.getPath(), ASSIMP_POST);
      AiMesh mesh = scene.getMeshes().get(meshId);
      return loadData(mesh, null);
  }
  
  private static Mesh loadData(AiMesh mesh, AiMaterial material) {
    float[] vertices = new float[mesh.getNumVertices() * 3];
    float[] textureCoords = new float[mesh.getNumVertices() * 2];
    float[] normals = new float[mesh.getNumVertices() * 3];
    int[] indices = new int[mesh.getNumFaces() * 3];

    int counter = 0;
    for (int v = 0; v < mesh.getNumVertices(); v++) {
        vertices[counter++] = mesh.getPositionX(v);
        vertices[counter++] = mesh.getPositionY(v);
        vertices[counter++] = mesh.getPositionZ(v);
    }
    counter = 0;
    for (int t = 0; t < mesh.getNumVertices(); t++) {
        textureCoords[counter++] = mesh.getTexCoordU(t, 0);
        textureCoords[counter++] = mesh.getTexCoordV(t, 0);

    }
    counter = 0;
    for (int n = 0; n < mesh.getNumVertices(); n++) {
        normals[counter++] = mesh.getNormalX(n);
        normals[counter++] = mesh.getNormalY(n);
        normals[counter++] = mesh.getNormalZ(n);
    }

    counter = 0;
    for (int f = 0; f < mesh.getNumFaces(); f++) {
        indices[counter++] = mesh.getFaceVertex(f, 0);
        indices[counter++] = mesh.getFaceVertex(f, 1);
        indices[counter++] = mesh.getFaceVertex(f, 2);
    }
    return new Mesh(vertices, textureCoords, normals, indices, 0, material);
}

public static class Mesh {

    protected AiMaterial material;

    protected float[] vertices;
    protected float[] textureCoords;
    protected float[] normals;
    protected int[] indices;
    protected float furthestPoint;

    public Mesh(float[] vertices, float[] textureCoords, float[] normals, int[] indices, float furthestPoint, AiMaterial material) {
        this.vertices = vertices;
        this.textureCoords = textureCoords;
        this.normals = normals;
        this.indices = indices;
        this.furthestPoint = furthestPoint;
        this.material = material;
    }

    public Mesh(float[] vertices, float[] textureCoords, float[] normals, int[] indices, float furthestPoint) {
        this(vertices, textureCoords, normals, indices, furthestPoint, null);
    }

    public float[] getVertices() {
        return vertices;
    }

    public float[] getTextureCoords() {
        return textureCoords;
    }

    public float[] getNormals() {
        return normals;
    }

    public int[] getIndices() {
        return indices;
    }

    public float getFurthestPoint() {
        return furthestPoint;
    }

    public AiMaterial getMaterial() {
        return material;
    }
}
```

Native: https://github.com/assimp/assimp
