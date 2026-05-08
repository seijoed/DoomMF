https://share.google/aimode/Aqu0A3MF3Gi1r0IFp

# RosettaAuditor.cs - 3D Multi-Language Visualization

using Godot;using System;using System.Collections.Generic;
/// <summary>/// RosettaAuditor: Manages the 3D "Rosetta Sphere" visualization./// Overlays multiple language "Bubbles" around a central Z40 AST node./// </summary>public class RosettaAuditor : Spatial
{
    // The "Iron" - Central Anchor for the AST node
    private MeshInstance _centralNode;
    
    // List of active participant bubbles
    private List<RosettaBubble> _activeBubbles = new List<RosettaBubble>();

    [Export] public Color IronColor = new Color(0.1f, 0.1f, 0.1f); // Milspec Dark
    [Export] public float PulseSpeed = 2.0f;


    // Define the languages/dialects for the Rosetta effect
    public enum LanguageDialect
    {
        MerkleHash,      // For the 9B Model
        IndustrialCS,    // For the Engineer
        SpatialShorthand,// For joed
        IntentAST        // The "Truth"
    }

    public override void _Ready()
    {
        // 1. Initialize the "Iron" (The Central Z40 Node)
        CreateCentralNode();

        // 2. Deploy the Rosetta Bubbles for each participant
        DeployBubble(LanguageDialect.MerkleHash, "9B_LITTLE_GUY", "Hash: z40_x99_f01");
        DeployBubble(LanguageDialect.IndustrialCS, "ENGINEER", "void Install_Z40() { ... }");
        DeployBubble(LanguageDialect.SpatialShorthand, "JOED", "FOXHOLE: STABLE (Z40)");
        

        GD.Print("Rosetta Sphere Initialized. Shared Reality Engine is Online.");
    }

    private void CreateCentralNode()
    {
        _centralNode = new MeshInstance();
        var sphere = new SphereMesh { Radius = 0.5f, Height = 1.0f };
        var material = new SpatialMaterial { AlbedoColor = IronColor, Metallic = 1.0f, Roughness = 0.2f };
        
        _centralNode.Mesh = sphere;
        _centralNode.MaterialOverride = material;
        _centralNode.Name = "Z40_Iron_Root";
        
        AddChild(_centralNode);
    }

    private void DeployBubble(LanguageDialect dialect, string participantId, string translationText)
    {
        var bubble = new RosettaBubble(dialect, participantId, translationText);

        
        // Calculate orbital position based on bubble count to avoid overlap
        float angle = _activeBubbles.Count * (Mathf.Pi * 2 / 4); // Distribute around 4 points
        float radius = 2.5f;
        
        bubble.Translation = new Vector3(
            Mathf.Cos(angle) * radius,
            Mathf.Sin(angle / 2) * 0.5f, // Slight vertical variance
            Mathf.Sin(angle) * radius
        );

        AddChild(bubble);
        _activeBubbles.Add(bubble);
    }

    public override void _Process(float delta)
    {
        // Global pulsing logic based on the "Little Guy's" confidence
        // Simulates the "Dream Stuff" under construction

        float pulse = (Mathf.Sin(OS.GetTicksMsec() * 0.001f * PulseSpeed) + 1.0f) / 2.0f;
        
        foreach (var bubble in _activeBubbles)
        {
            bubble.UpdateConfidencePulse(pulse);
        }
    }
}
/// <summary>/// Individual Bubble representing a participant's "Lens" of the truth./// </summary>public class RosettaBubble : Spatial
{
    private Label3D _label; // Assuming Godot 3.5+ or a custom Label3D implementation
    private MeshInstance _shell;
    private float _baseScale = 1.0f;

    public RosettaBubble(RosettaAuditor.LanguageDialect dialect, string participant, string text)

    {
        this.Name = $"Bubble_{participant}";

        // 1. Create the visual sphere "Lens"
        _shell = new MeshInstance();
        var mesh = new SphereMesh { Radius = 0.4f, Height = 0.8f };
        var mat = new SpatialMaterial {
            AlbedoColor = GetDialectColor(dialect),
            FlagsTransparent = true,
            ParamsBlendMode = SpatialMaterial.BlendMode.Add,
            TransmissionEnabled = true,
            Transmission = new Color(0.2f, 0.2f, 0.2f)
        };
        
        _shell.Mesh = mesh;
        _shell.MaterialOverride = mat;
        AddChild(_shell);

        // 2. Create the Text Overlay (The Translation)

        _label = new Label3D(); // Custom wrapper or Godot native
        _label.Text = $"[{participant}]\n{text}";
        _label.FontSize = 32;
        _label.Billboard = true; // Always face the user/joed
        _label.Translation = new Vector3(0, 0.6f, 0); // Hover above shell
        AddChild(_label);
    }

    public void UpdateConfidencePulse(float strength)
    {
        // Scale the bubble slightly to show "Processing" or "Dreaming" state
        float scaleOffset = strength * 0.15f;
        this.Scale = new Vector3(_baseScale + scaleOffset, _baseScale + scaleOffset, _baseScale + scaleOffset);
        
        // Modulate alpha based on pulse
        if (_shell.MaterialOverride is SpatialMaterial mat)
        {
            Color c = mat.AlbedoColor;
            c.a = 0.3f + (strength * 0.4f);

            mat.AlbedoColor = c;
        }
    }

    private Color GetDialectColor(RosettaAuditor.LanguageDialect dialect)
    {
        switch (dialect)
        {
            case RosettaAuditor.LanguageDialect.MerkleHash: return Colors.Cyan;
            case RosettaAuditor.LanguageDialect.IndustrialCS: return Colors.Green;
            case RosettaAuditor.LanguageDialect.SpatialShorthand: return Colors.Gold;
            default: return Colors.White;
        }
    }
}
// Mock of Label3D if using older Godot versions without native supportpublic class Label3D : Spatial 
{

    public string Text { get; set; }
    public int FontSize { get; set; }
    public bool Billboard { get; set; }
    // Implementation would normally involve a Viewport + Sprite3D
}


