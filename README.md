
import React, { useState, useEffect } from 'react';
import {
  Calculator, Home, Wrench, FileText, DollarSign, Lightbulb,
  Shield, Calendar, Users, Globe, Mic, Eye, EyeOff, Minus
} from 'lucide-react';

const Manda1ContractorsEstimator = () => {
  const [activeTab, setActiveTab] = useState('overview');
  const [units, setUnits] = useState('imperial');
  const [language, setLanguage] = useState('en');
  const [projectData, setProjectData] = useState({
    projectName: '',
    clientName: '',
    address: '',
    startDate: '',
    estimatedDays: '',
    laborRate: 25
  });
  const [categories, setCategories] = useState({
    materials: [],
    labor: [],
    fixtures: [],
    landscaping: []
  });
  const [showAI, setShowAI] = useState(false);
  const [safetyChecklist, setSafetyChecklist] = useState([]);

  useEffect(() => {
    try {
      const msg = new SpeechSynthesisUtterance(
        "Welcome to Manda One. Taking the stress out of estimates."
      );
      window.speechSynthesis.speak(msg);
    } catch (e) {}
  }, []);

  const laborCategories = [
    { name: "Demolition", rate: 30 },
    { name: "Drywall Installation", rate: 40 },
    { name: "Painting", rate: 35 },
    { name: "Tiling", rate: 50 },
    { name: "Plumbing", rate: 85 },
    { name: "Electrical", rate: 75 },
  ];

  const materialLibrary = {
    flooring: {
      'Ceramic Tiles': {
        unit: 'sq ft', price: 3.50,
        sources: ['Home Depot: $3.50', 'Lowes: $3.45']
      },
      'Hardwood Oak': {
        unit: 'sq ft', price: 8.99,
        sources: ['Lumber Liquidators: $8.99']
      },
    },
    paint: {
      'Interior Paint (Premium)': {
        unit: 'gallon', price: 45,
        sources: ['Sherwin Williams: $45']
      },
      'Primer': {
        unit: 'gallon', price: 35,
        sources: ['Home Depot: $35']
      }
    }
  };

  const fixturesLibrary = {
    lighting: {
      'LED Ceiling Light': {
        unit: 'each', price: 65,
        sources: ['Home Depot: $65']
      },
      'Chandelier': {
        unit: 'each', price: 350,
        sources: ['Wayfair: $320']
      },
    }
  };

  const landscapingMaterials = {
    grass: {
      'Bermuda Sod': {
        unit: 'sq ft', price: 0.45,
        sources: ['Local Nursery: $0.45']
      },
    },
    plants: {
      'Oak Tree (6ft)': {
        unit: 'each', price: 150,
        sources: ['Local Nursery: $150']
      }
    }
  };

  const safetyItems = [
    'Hard Hat','Safety Glasses','Work Gloves',
    'Steel-toe Boots','Hi-vis Vest','Respirator/Mask'
  ];

  const translations = {
    en: {
      welcome: "Welcome to Manda One",
      slogan: "Taking the stress out of estimates",
      labor: "Labor", materials: "Materials",
      calculate: "Calculate", overview: "Overview",
      safety: "Safety Check", calendar: "Calendar",
      profile: "Profile"
    },
    es: {
      welcome: "Bienvenido a Manda One",
      slogan: "Quitamos el estrés de las estimaciones",
      labor: "Trabajo", materials: "Materiales",
      calculate: "Calcular", overview: "Resumen",
      safety: "Verificación de Seguridad", calendar: "Calendario",
      profile: "Perfil"
    }
  };

  const getCurrentTranslation = () => translations[language] || translations.en;

  const addItem = (category, item) => {
    setCategories(prev => ({
      ...prev,
      [category]: [...(prev[category] || []), { ...item, id: Date.now() }]
    }));
  };

  const removeItem = (category, id) => {
    setCategories(prev => ({
      ...prev,
      [category]: prev[category].filter(i => i.id !== id)
    }));
  };

  const updateQuantity = (category, id, quantity) => {
    setCategories(prev => ({
      ...prev,
      [category]: prev[category].map(item =>
        item.id === id ? { ...item, quantity: parseFloat(quantity) || 0 } : item
      )
    }));
  };

  const calculateTotal = () => {
    let total = 0;
    Object.values(categories).forEach(cat => {
      cat.forEach(item => total += (item.quantity || 0) * (item.price || 0));
    });
    return total;
  };

  const speakText = (text) => {
    try {
      const u = new SpeechSynthesisUtterance(text);
      window.speechSynthesis.speak(u);
    } catch (e) {}
  };

  const AIAssistant = () => (
    <div className="bg-gradient-to-r from-blue-50 to-purple-50 p-4 rounded border">
      <h3 className="font-semibold mb-2 flex items-center">
        <Lightbulb className="mr-2" />AI Construction Assistant
      </h3>
      <div className="space-y-2">
        <div className="bg-white p-2 rounded border-l-4 border-blue-400">
          For bathroom renovations, consider waterproof flooring and adequate ventilation.
        </div>
        <div className="bg-white p-2 rounded border-l-4 border-green-400">
          Add 10% extra materials for waste and cuts.
        </div>
        <button
          onClick={() => speakText("Welcome to Manda One AI Assistant. How can I help with your construction project today?")}
          className="mt-2 bg-purple-600 text-white px-3 py-1 rounded flex items-center"
        >
          <Mic className="mr-2" />Ask Manda AI
        </button>
      </div>
    </div>
  );

  const SafetyCheck = () => (
    <div className="bg-yellow-50 p-4 rounded border">
      <h3 className="font-semibold mb-2 flex items-center">
        <Shield className="mr-2" />Daily Safety Checklist
      </h3>
      <div className="grid grid-cols-2 gap-2">
        {safetyItems.map(item => (
          <label key={item} className="flex items-center space-x-2">
            <input
              type="checkbox"
              className="rounded"
              onChange={(e) =>
                setSafetyChecklist(prev =>
                  e.target.checked
                    ? [...prev, item]
                    : prev.filter(i => i !== item)
                )
              }
            />
            <span className="text-sm">{item}</span>
          </label>
        ))}
      </div>
      <p className="mt-2">
        Score: {safetyChecklist.length}/{safetyItems.length} (
        {Math.round((safetyChecklist.length / safetyItems.length) * 100)}%)
      </p>
    </div>
  );

  const MaterialSelector = ({ category, materials }) => (
    <div className="space-y-3">
      {Object.entries(materials).map(([subcategory, items]) => (
        <div key={subcategory} className="border rounded p-3">
          <h4 className="font-semibold capitalize mb-2">{subcategory}</h4>
          <div className="space-y-2">
            {Object.entries(items).map(([name, details]) => (
              <div
                key={name}
                className="flex items-center justify-between bg-gray-50 p-2 rounded"
              >
                <div>
                  <div className="font-medium">{name}</div>
                  <div className="text-sm text-gray-600">
                    ${details.price} per {details.unit}
                  </div>
                </div>
                <button
                  onClick={() =>
                    addItem(category, {
                      name,
                      price: details.price,
                      unit: details.unit,
                      quantity: 1
                    })
                  }
                  className="bg-blue-500 text-white px-2 py-1 rounded"
                >
                  Add
                </button>
              </div>
            ))}
          </div>
        </div>
      ))}
    </div>
  );

  return (
    <div className="max-w-6xl mx-auto p-4 bg-white rounded shadow">
      <div className="bg-gradient-to-r from-blue-600 to-purple-600 text-white p-4 rounded mb-4">
        <div className="flex justify-between items-start">
          <div>
            <h1 className="text-2xl font-bold">MANDA1</h1>
            <div className="text-sm">{getCurrentTranslation().slogan}</div>
          </div>
          <div className="flex space-x-2">
            <select
              value={language}
              onChange={(e) => setLanguage(e.target.value)}
              className="rounded px-2 py-1 bg-white/20 text-white"
            >
              <option value="en">English</option>
              <option value="es">Español</option>
            </select>
            <select
              value={units}
              onChange={(e) => setUnits(e.target.value)}
              className="rounded px-2 py-1 bg-white/20 text-white"
            >
              <option value="imperial">Imperial</option>
              <option value="metric">Metric</option>
            </select>
          </div>
        </div>
      </div>

      {/* Project Info */}
      <div className="p-4 border rounded mb-4">
        <h2 className="font-semibold mb-2">Project Information</h2>
        <div className="grid md:grid-cols-2 gap-2">
          <input
            placeholder="Project Name"
            value={projectData.projectName}
            onChange={(e) =>
              setProjectData(prev => ({ ...prev, projectName: e.target.value }))
            }
            className="border rounded px-2 py-1"
          />
          <input
            placeholder="Client Name"
            value={projectData.clientName}
            onChange={(e) =>
              setProjectData(prev => ({ ...prev, clientName: e.target.value }))
            }
            className="border rounded px-2 py-1"
          />
          <input
            placeholder="Project Address"
            value={projectData.address}
            onChange={(e) =>
              setProjectData(prev => ({ ...prev, address: e.target.value }))
            }
            className="border rounded px-2 py-1"
          />
          <input
            type="date"
            value={projectData.startDate}
            onChange={(e) =>
              setProjectData(prev => ({ ...prev, startDate: e.target.value }))
            }
            className="border rounded px-2 py-1"
          />
        </div>
      </div>

      {/* Tabs */}
      <div className="mb-4 bg-white rounded border">
        <div className="flex flex-wrap border-b overflow-x-auto">
          {[
            { id: 'overview', label: getCurrentTranslation().overview, icon: FileText },
            { id: 'materials', label: getCurrentTranslation().materials, icon: Home },
            { id: 'labor', label: getCurrentTranslation().labor, icon: Wrench },
            { id: 'fixtures', label: 'Fixtures', icon: Lightbulb },
            { id: 'landscaping', label: 'Landscaping', icon: Globe },
            { id: 'safety', label: getCurrentTranslation().safety, icon: Shield },
            { id: 'calendar', label: getCurrentTranslation().calendar, icon: Calendar },
            { id: 'profile', label: getCurrentTranslation().profile, icon: Users },
            { id: 'inspiration', label: 'Inspiration', icon: Globe }
          ].map(tab => (
            <button
              key={tab.id}
              onClick={() => setActiveTab(tab.id)}
              className={`px-4 py-3 ${activeTab === tab.id
                ? 'border-b-2 border-blue-500 text-blue-600 bg-blue-50'
                : 'text-gray-600 hover:bg-gray-50'
                }`}
            >
              <tab.icon size={14} className="inline-block mr-2" /> {tab.label}
            </button>
          ))}
        </div>

        <div className="p-4">
          <div className="mb-4">
            <button
              onClick={() => setShowAI(!showAI)}
              className="text-blue-600"
            >
              {showAI ? (
                <>
                  <EyeOff size={14} /> Hide AI
                </>
              ) : (
                <>
                  <Eye size={14} /> Show AI
                </>
              )}
            </button>
            {showAI && <div className="mt-3"><AIAssistant /></div>}
          </div>

          {activeTab === 'overview' && (
            <div className="space-y-4">
              <div className="grid md:grid-cols-2 gap-4">
                <div className="bg-green-50 p-4 rounded">
                  <h3 className="font-semibold flex items-center">
                    <DollarSign className="mr-2" />Project Summary
                  </h3>
                  <p>Total Estimate: ${calculateTotal().toLocaleString()}</p>
                  <p>
                    Materials: $
                    {(categories.materials || []).reduce((s, i) =>
                      s + (i.quantity || 0) * (i.price || 0), 0
                    ).toLocaleString()}
                  </p>
                  <p>
                    Labor: $
                    {(categories.labor || []).reduce((s, i) =>
                      s + (i.quantity || 0) * (i.price || 0), 0
                    ).toLocaleString()}
                  </p>
                  <button
                    onClick={() =>
                      speakText(`Total project estimate is ${calculateTotal().toLocaleString()} dollars`)
                    }
                    className="mt-2 bg-green-600 text-white px-3 py-1 rounded flex items-center"
                  >
                    <Mic className="mr-2" />Speak Total
                  </button>
                </div>
                <div className="bg-yellow-50 p-4 rounded">
                  <h3 className="font-semibold">Quick Calculator</h3>
                  <p className="text-sm">
                    Simple area calculator available in-app.
                  </p>
                  <button
                    className="mt-2 bg-blue-500 text-white px-3 py-1 rounded flex items-center"
                  >
                    <Calculator className="mr-2" />{getCurrentTranslation().calculate}
                  </button>
                </div>
              </div>
              <SafetyCheck />
            </div>
          )}

          {activeTab === 'materials' && (
            <div className="space-y-4">
              <h3 className="font-semibold">Materials Library</h3>
              <MaterialSelector category="materials" materials={materialLibrary} />
              <div className="border rounded p-3">
                <h4 className="font-semibold mb-2">Selected Materials</h4>
                {categories.materials.length === 0 ? (
                  <p className="text-gray-500">No materials selected yet.</p>
                ) : categories.materials.map(item => (
                  <div
                    key={item.id}
                    className="flex items-center justify-between bg-gray-50 p-2 rounded mb-2"
                  >
                    <div className="flex-1 grid grid-cols-4 gap-2">
                      <span className="font-medium">{item.name}</span>
                      <input
                        type="number"
                        value={item.quantity}
                        onChange={(e) =>
                          updateQuantity('materials', item.id, e.target.value)
                        }
                        className="border rounded px-2 py-1"
                      />
                      <span className="text-sm">
                        ${item.price} per {item.unit}
                      </span>
                      <span className="font-medium">
                        ${((item.quantity || 0) * item.price).toFixed(2)}
                      </span>
                    </div>
                    <button
                      onClick={() => removeItem('materials', item.id)}
                      className="text-red-500"
                    >
                      <Minus />
                    </button>
                  </div>
                ))}
              </div>
            </div>
          )}

          {activeTab === 'labor' && (
            <div className="space-y-4">
              <h3 className="font-semibold">Labor Categories</h3>
              <div className="grid gap-2">
                {laborCategories.map(l => (
                  <div
                    key={l.name}
                    className="flex items-center justify-between bg-gray-50 p-2 rounded"
                  >
                    <div>
                      <p className="font-medium">{l.name}</p>
                      <p className="text-sm">${l.rate} per hour</p>
                    </div>
                    <button
                      onClick={() =>
                        addItem('labor', {
                          name: l.name, price: l.rate, unit: 'hour', quantity: 1
                        })
                      }
                      className="bg-green-500 text-white px-3 py-1 rounded"
                    >
                      Add Labor
                    </button>
                  </div>
                ))}
              </div>
            </div>
          )}

          {activeTab === 'fixtures' && (
            <div className="space-y-4">
              <h3 className="font-semibold">Fixtures & Hardware</h3>
              <MaterialSelector category="fixtures" materials={fixturesLibrary} />
            </div>
          )}

          {activeTab === 'landscaping' && (
            <div className="space-y-4">
              <h3 className="font-semibold">Landscaping</h3>
              <MaterialSelector category="landscaping" materials={landscapingMaterials} />
            </div>
          )}

          {activeTab === 'safety' && (
            <div className="space-y-4">
              <h3 className="font-semibold">Safety Management
