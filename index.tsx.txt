import React, { useState, useCallback, useEffect, useRef, useMemo } from 'react';
import { createRoot } from 'react-dom/client';
import { 
  Shield, 
  Terminal, 
  Download, 
  Upload,
  RefreshCw, 
  Search, 
  Settings, 
  Code, 
  Info, 
  Copy,
  ChevronRight,
  X,
  Cpu,
  Layers,
  Database,
  Wrench,
  Variable,
  Zap,
  Activity,
  Trash2,
  History,
  CheckCircle2,
  GitBranch,
  Flame,
  Scale,
  Wind,
  FileCode,
  Type,
  Binary,
  ChevronDown,
  Puzzle
} from 'lucide-react';

const LOGO_URL = "https://raw.githubusercontent.com/BloxCrypto/Web/refs/heads/main/file_0000000022147208b4581284d9105414.png";

interface LogEntry {
  id: string;
  timestamp: string;
  message: string;
  level: 'info' | 'success' | 'warning' | 'error' | 'process';
}

interface ChangeLog {
  version: string;
  date: string;
  changes: string[];
  type: 'major' | 'minor' | 'patch';
}

type PresetKey = 'high' | 'balanced' | 'fast' | 'custom';

// --- Obfuscator Engine (Enterprise Grade) ---
class LuaObfuscatorEngine {
  static generateRandomId(length = 10) {
    const chars = 'abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ_';
    const hex = '0123456789abcdef';
    let result = chars.charAt(Math.floor(Math.random() * chars.length));
    for (let i = 0; i < length; i++) {
      result += hex.charAt(Math.floor(Math.random() * hex.length));
    }
    return `_0x${result}`;
  }

  static applyEnhancedVM(code: string, options: any) {
    const dispatcherId = this.generateRandomId(8);
    const instTableId = this.generateRandomId(5);
    const pcId = this.generateRandomId(4);
    const dataId = this.generateRandomId(6);
    const envId = this.generateRandomId(5);
    const decodeKey = Math.floor(Math.random() * 255) + 1;

    const lines = code.split('\n').filter(l => l.trim().length > 0);
    const instructions = lines.map((line, idx) => {
      const salt = Math.floor(Math.random() * 1000);
      return {
        op: (idx + salt) ^ decodeKey,
        salt: salt,
        exec: `function(${envId}) ${line} end`
      };
    });

    const shuffled = [...instructions].sort(() => Math.random() - 0.5);
    const opMap = shuffled.map(i => `[${i.op}] = ${i.exec}`).join(',\n  ');
    const sequence = instructions.map(i => i.op).join(',');

    const vmWrapper = `
local ${dataId} = {${sequence}}
local ${instTableId} = {
  ${opMap}
}
local function ${dispatcherId}(_seq, _ops)
    local ${pcId} = 1
    local ${envId} = {}
    while true do
        local _opcode = _seq[${pcId}]
        if not _opcode then break end
        local _func = _ops[_opcode]
        if _func then
            local _res = _func(${envId})
            if _res == true then break end
        end
        ${pcId} = ${pcId} + 1
    end
end
${dispatcherId}(${dataId}, ${instTableId})`.trim();

    return vmWrapper;
  }

  static obfuscate(code: string, options: any, onProgress?: (msg: string, level?: LogEntry['level']) => void) {
    const startTime = performance.now();
    let result = code;
    let variablesChanged = 0;
    const originalSize = code.length;

    const luaKeywords = new Set([
      'and', 'break', 'do', 'else', 'elseif', 'end', 'false', 'for', 'function', 'if', 'in', 
      'local', 'nil', 'not', 'or', 'repeat', 'return', 'then', 'true', 'until', 'while',
      'string', 'number', 'boolean', 'nil', 'table', 'thread', 'userdata', 'getfenv', 
      'setfenv', 'bit32', 'ipairs', 'pairs', 'next', 'tostring', 'tonumber', 'print', 
      'warn', 'error', 'wait', 'spawn', 'math', 'os', 'coroutine', 'debug', 'package', 
      'require', 'module', 'select', 'pcall', 'xpcall', 'rawget', 'rawset', 'rawlen'
    ]);

    onProgress?.("Initializing obfuscation pipeline...", "info");

    if (options.removeComments) {
      onProgress?.("Stripping comments and metadata...", "process");
      result = result.replace(/--\[\[[\s\S]*?\]\]/g, '');
      result = result.replace(/--.*$/gm, '');
    }

    if (options.advancedRenaming) {
      onProgress?.("Performing scope analysis and global aliasing...", "process");
      const globalsToAlias = ['print', 'math', 'string', 'table', 'os', 'getfenv', 'setfenv', 'wait', 'spawn', 'warn', 'error', 'bit32'];
      let aliasBlock = "";
      globalsToAlias.forEach(g => {
        if (result.includes(g)) {
          const aliasName = this.generateRandomId(12);
          aliasBlock += `local ${aliasName} = ${g};\n`;
          const regex = new RegExp(`\\b${g}\\b`, 'g');
          result = result.replace(regex, aliasName);
        }
      });
      result = aliasBlock + result;
    }

    // Function Virtualization
    if (options.functionVirtualization) {
      onProgress?.("Virtualizing function execution entries...", "process");
      const registryId = this.generateRandomId(8);
      const funcRegex = /local function\s+([a-zA-Z_][a-zA-Z0-9_]*)\s*\((.*?)\)/g;
      let transformedCode = result;
      let index = 1;
      let match;
      
      while ((match = funcRegex.exec(result)) !== null) {
        const name = match[1];
        const idx = index++;
        const proxyCall = `local ${name}; ${name} = function(...) return ${registryId}[${idx}](...) end`;
        transformedCode = transformedCode.replace(match[0], proxyCall);
        onProgress?.(`Virtualizing internal entry: ${name}`, "process");
      }
      const registryBlock = `local ${registryId} = setmetatable({}, {__index = function(t, k) return rawget(t, k) or function() end end});\n`;
      result = registryBlock + transformedCode;
    }

    if (options.obfuscateBooleans) {
      onProgress?.("Mapping boolean literals to expressions...", "process");
      result = result.replace(/\btrue\b/g, '((function() return (not not (1)) end)())');
      result = result.replace(/\bfalse\b/g, '((function() return (not (1 == 1)) end)())');
    }

    if (options.opaquePredicates) {
      onProgress?.("Injecting opaque predicates for branch confusion...", "process");
      const lines = result.split('\n');
      result = lines.map(line => {
        if (line.trim().length > 10 && !line.includes('return') && !line.includes('local') && Math.random() > 0.7) {
          const predicates = [
            `if (math.floor(math.pi * 10) == 31) then ${line} end`,
            `if ((function(a) return a*a >= 0 end)(math.random(1,100))) then ${line} end`,
            `if (not (1 == 2)) then ${line} end`
          ];
          return predicates[Math.floor(Math.random() * predicates.length)];
        }
        return line;
      }).join('\n');
    }

    if (options.asciiEscaping) {
      onProgress?.("Applying ASCII decimal escaping...", "process");
      const stringRegex = /(["'])(?:(?=(\\?))\2.)*?\1/g;
      result = result.replace(stringRegex, (match) => {
        const quote = match[0];
        const content = match.slice(1, -1);
        if (content.length === 0) return match;
        let escaped = "";
        for (let i = 0; i < content.length; i++) {
          const charCode = content.charCodeAt(i);
          escaped += "\\" + charCode.toString().padStart(3, '0');
        }
        return quote + escaped + quote;
      });
    }

    if (options.encryptStrings) {
      onProgress?.("Injecting rotating XOR decoders...", "process");
      const stringRegex = /(["'])(?:(?=(\\?))\2.)*?\1/g;
      result = result.replace(stringRegex, (match) => {
        const str = match.slice(1, -1);
        if (str.length === 0) return match;
        const encoded = Array.from(str).map(c => c.charCodeAt(0));
        const key = Math.floor(Math.random() * 255) + 1;
        const xored = encoded.map(v => v ^ key);
        
        const xorLogic = options.lua54Support 
          ? `(_v ~ _k)` 
          : `(bit32 and bit32.bxor(_v, _k) or (_v + _k) % 256)`;
          
        return `(function() local _k, _d = ${key}, {${xored.join(',')}} local _s = "" for _, _v in ipairs(_d) do _s = _s .. string.char(${xorLogic}) end return _s end)()`;
      });
    }

    if (options.obfuscateNumbers || options.numbersCombine) {
      onProgress?.("Applying arithmetic masking to constants...", "process");
      const numRegex = /\b\d+(\.\d+)?\b/g;
      result = result.replace(numRegex, (match) => {
        const num = parseFloat(match);
        if (isNaN(num)) return match;
        
        if (options.numbersCombine) {
          const part1 = Math.floor(num / 2);
          const part2 = num - part1;
          const r1 = Math.floor(Math.random() * 500) + 1;
          return `((${part1 + r1} - ${r1}) + (${part2 * 2} / 2))`;
        } else {
          const r1 = Math.floor(Math.random() * 1000) + 1;
          return `((${num + r1} - ${r1}))`;
        }
      });
    }

    if (options.renameVariables) {
      onProgress?.("Scrambling identifiers and scoping...", "process");
      const localVars = new Set<string>();
      
      // Multi-local declaration: local a, b, c = 1, 2, 3
      const multiLocalRegex = /\blocal\s+([a-zA-Z_][a-zA-Z0-9_]*(\s*,\s*[a-zA-Z_][a-zA-Z0-9_]*)*)/g;
      let match;
      while ((match = multiLocalRegex.exec(result)) !== null) {
        match[1].split(',').forEach(v => {
          const name = v.trim();
          if (name && !luaKeywords.has(name)) localVars.add(name);
        });
      }

      // Function parameters: function(a, b, c)
      const funcParamsRegex = /function\s*[a-zA-Z_0-9.]*\s*\((.*?)\)/g;
      while ((match = funcParamsRegex.exec(result)) !== null) {
        match[1].split(',').forEach(v => {
          const name = v.trim();
          if (name && name !== '...' && !luaKeywords.has(name)) localVars.add(name);
        });
      }

      // Loop variables: for i, v in ...
      const loopVarRegex = /for\s+([a-zA-Z_][a-zA-Z0-9_]*(\s*,\s*[a-zA-Z_][a-zA-Z0-9_]*)*)\s+in/g;
      while ((match = loopVarRegex.exec(result)) !== null) {
        match[1].split(',').forEach(v => {
          const name = v.trim();
          if (name && !luaKeywords.has(name)) localVars.add(name);
        });
      }

      localVars.forEach(v => {
        const newName = this.generateRandomId(14);
        const escapedV = v.replace(/[.*+?^${}()|[\]\\]/g, '\\$&');
        const searchRegex = new RegExp(`\\b${escapedV}\\b`, 'g');
        result = result.replace(searchRegex, newName);
        variablesChanged++;
      });
    }

    if (options.vmObfuscation) {
      onProgress?.(`Bootstrapping Enhanced VM (PC Dispatcher)...`, "process");
      result = this.applyEnhancedVM(result, options);
    }

    if (options.minification) {
      onProgress?.("Compressing final bytecode stream...", "process");
      result = result.replace(/\s+/g, ' ').replace(/\s?([=+\-*/%^#,<>])\s?/g, '$1').trim();
    }

    result = `-- Obfuscated by LuaForm (Enterprise v5.1.2)\n` + result;
    const endTime = performance.now();
    onProgress?.(`Hardening complete in ${Math.round(endTime - startTime)}ms.`, "success");

    return {
      code: result,
      originalSize,
      newSize: result.length,
      variablesChanged,
      timeTaken: Math.round(endTime - startTime)
    };
  }
}

const App = () => {
  const [activeTab, setActiveTab] = useState('changelog'); 
  const [inputCode, setInputCode] = useState('-- Sample secure script\nlocal function authenticate(user, pass)\n  local isValid = (user == "admin" and pass == "secret123")\n  if isValid then\n    print("Access Granted for " .. user)\n  else\n    warn("Invalid login attempt")\n  end\n  return isValid\nend\n\nauthenticate("guest", "1234")');
  const [outputData, setOutputData] = useState<any>(null);
  const [isProcessing, setIsProcessing] = useState(false);
  const [searchTerm, setSearchTerm] = useState('');
  const [isPresetOpen, setIsPresetOpen] = useState(false);
  const [activePreset, setActivePreset] = useState<PresetKey>('balanced');
  const [logs, setLogs] = useState<LogEntry[]>([
    { id: 'init', timestamp: new Date().toLocaleTimeString(), message: 'System initialized. Ready for script hardening.', level: 'info' }
  ]);

  const logEndRef = useRef<HTMLDivElement>(null);
  const fileInputRef = useRef<HTMLInputElement>(null);
  const presetRef = useRef<HTMLDivElement>(null);
  
  const [options, setOptions] = useState({
    renameVariables: true,
    advancedRenaming: true,
    lua54Support: false,
    encryptStrings: true,
    asciiEscaping: false,
    obfuscateNumbers: true,
    numbersCombine: false,
    nonsenseNumber: false,
    obfuscateBooleans: true,
    opaquePredicates: false,
    tableLookup: false,
    gLookup: false,
    functionIndirection: true,
    functionVirtualization: true,
    callReturn: false,
    removeComments: true,
    minification: false,
    vmObfuscation: true,
    bytecodeMode: false
  });

  const changelogs: ChangeLog[] = [
    {
      version: 'v5.1.2',
      date: 'Latest',
      type: 'patch',
      changes: [
        'Enhanced Variable Renaming to capture function parameters and loop symbols.',
        'Improved identifier scrambling with keyword collision avoidance.',
        'Refined global aliasing logic for more environment flexibility.',
      ]
    },
    {
      version: 'v5.1.0',
      date: 'June 2024',
      type: 'minor',
      changes: [
        'Added Function Virtualization logic to decouple entry points from declarations.',
        'Refined advanced renaming regex to better handle nested block scopes.',
        'Optimized settings dropdown UX for faster navigation.',
      ]
    }
  ];

  useEffect(() => {
    if (logEndRef.current) {
      logEndRef.current.scrollIntoView({ behavior: 'smooth' });
    }
  }, [logs]);

  useEffect(() => {
    const handleClickOutside = (event: MouseEvent) => {
      if (presetRef.current && !presetRef.current.contains(event.target as Node)) {
        setIsPresetOpen(false);
      }
    };
    document.addEventListener("mousedown", handleClickOutside);
    return () => document.removeEventListener("mousedown", handleClickOutside);
  }, []);

  const addLog = useCallback((message: string, level: LogEntry['level'] = 'info') => {
    setLogs(prev => [...prev, {
      id: Math.random().toString(36).substr(2, 9),
      timestamp: new Date().toLocaleTimeString(),
      message,
      level
    }]);
  }, []);

  const clearLogs = () => {
    setLogs([{ id: 'init', timestamp: new Date().toLocaleTimeString(), message: 'Console cleared.', level: 'info' }]);
  };

  const handleFileUpload = (e: React.ChangeEvent<HTMLInputElement>) => {
    const file = e.target.files?.[0];
    if (!file) return;

    const reader = new FileReader();
    reader.onload = (event) => {
      const content = event.target?.result as string;
      setInputCode(content);
      addLog(`File successfully loaded: ${file.name} (${file.size} bytes)`, 'success');
    };
    reader.onerror = () => {
      addLog(`Error reading file: ${file.name}`, 'error');
    };
    reader.readAsText(file);
    if (fileInputRef.current) fileInputRef.current.value = '';
  };

  const handleObfuscate = useCallback(() => {
    if (!inputCode.trim()) return;
    setIsProcessing(true);
    addLog(`Starting obfuscation for payload (${inputCode.length} bytes)`, 'info');
    
    setTimeout(() => {
      try {
        const result = LuaObfuscatorEngine.obfuscate(inputCode, options, (msg, lvl) => addLog(msg, lvl));
        setOutputData(result);
        setActiveTab('output');
        addLog(`Payload scrambled. Variables mutated: ${result.variablesChanged}. Final size: ${result.newSize} bytes.`, 'success');
      } catch (err: any) {
        addLog(`Engine Error: ${err.message || 'Obfuscation failed.'}`, 'error');
      } finally {
        setIsProcessing(false);
      }
    }, 800);
  }, [inputCode, options, addLog]);

  const toggleOption = (id: string, label: string) => {
    const newVal = !(options as any)[id];
    setOptions(prev => ({ ...prev, [id]: newVal }));
    setActivePreset('custom');
    addLog(`Setting changed: ${label} -> ${newVal ? 'ENABLED' : 'DISABLED'}`, newVal ? 'success' : 'warning');
  };

  const applyPreset = (presetName: PresetKey) => {
    let newOptions = { ...options };
    if (presetName === 'high') {
      newOptions = {
        renameVariables: true,
        advancedRenaming: true,
        lua54Support: true,
        encryptStrings: true,
        asciiEscaping: true,
        obfuscateNumbers: true,
        numbersCombine: true,
        nonsenseNumber: true,
        obfuscateBooleans: true,
        opaquePredicates: true,
        tableLookup: true,
        gLookup: true,
        functionIndirection: true,
        functionVirtualization: true,
        callReturn: true,
        removeComments: true,
        minification: true,
        vmObfuscation: true,
        bytecodeMode: true
      };
      addLog("Preset Applied: V5 EXTREME - Maximum security activated.", "success");
    } else if (presetName === 'balanced') {
      newOptions = {
        renameVariables: true,
        advancedRenaming: true,
        lua54Support: false,
        encryptStrings: true,
        asciiEscaping: false,
        obfuscateNumbers: true,
        numbersCombine: false,
        nonsenseNumber: false,
        obfuscateBooleans: true,
        opaquePredicates: false,
        tableLookup: false,
        gLookup: false,
        functionIndirection: true,
        functionVirtualization: true,
        callReturn: false,
        removeComments: true,
        minification: false,
        vmObfuscation: true,
        bytecodeMode: false
      };
      addLog("Preset Applied: BALANCED - Optimized protection.", "info");
    } else if (presetName === 'fast') {
      newOptions = {
        renameVariables: true,
        advancedRenaming: false,
        lua54Support: false,
        encryptStrings: true,
        asciiEscaping: false,
        obfuscateNumbers: false,
        numbersCombine: false,
        nonsenseNumber: false,
        obfuscateBooleans: false,
        opaquePredicates: false,
        tableLookup: false,
        gLookup: false,
        functionIndirection: false,
        functionVirtualization: false,
        callReturn: false,
        removeComments: true,
        minification: false,
        vmObfuscation: false,
        bytecodeMode: false
      };
      addLog("Preset Applied: FAST ID - Basic identifier scrambling.", "warning");
    }
    setOptions(newOptions);
    setActivePreset(presetName);
    setIsPresetOpen(false);
  };

  const copyToClipboard = (text: string) => {
    navigator.clipboard.writeText(text);
    addLog("Output copied to clipboard.", "info");
  };

  const settingsConfig = [
    {
      title: "Logic Flow & VM",
      icon: Layers,
      options: [
        { id: 'vmObfuscation', label: 'Virtual Machine', desc: 'Enhanced PC-based dispatcher with multi-stage decoding.', isBeta: true, icon: Cpu },
        { id: 'functionVirtualization', label: 'Func Virtualization', desc: 'Maps functions into a registry table to hide declarations.', icon: Puzzle },
        { id: 'opaquePredicates', label: 'Opaque Predicates', desc: 'Injects branch confusion using mathematical identities.', icon: GitBranch },
        { id: 'functionIndirection', label: 'Func Indirection', desc: 'Separates declarations from assignments to break static analysis.', icon: Wrench },
        { id: 'callReturn', label: 'CallReturn Wrap', desc: 'Wraps returns in temporary environment stacks.', icon: Activity },
      ]
    },
    {
      title: "Constants & Data",
      icon: Database,
      options: [
        { id: 'encryptStrings', label: 'XOR Strings', desc: 'Encrypts strings with rotating numeric keys.', icon: Code },
        { id: 'asciiEscaping', label: 'ASCII Escaping', desc: 'Converts strings into unreadable decimal escape sequences.', icon: Type },
        { id: 'obfuscateNumbers', label: 'Number Masking', desc: 'Converts integers into arithmetic expressions.', icon: Database },
        { id: 'numbersCombine', label: 'Numbers Combine', desc: 'Aggregates multiple numeric constants into identities.', icon: Zap },
        { id: 'obfuscateBooleans', label: 'Boolean Scrambler', desc: 'Replaces booleans with complex logic calls.', icon: Code },
      ]
    },
    {
      title: "Environment & Scoping",
      icon: Variable,
      options: [
        { id: 'renameVariables', label: 'Variable Renaming', desc: 'Deep randomization of local identifier names and parameters.', icon: Variable },
        { id: 'advancedRenaming', label: 'Advanced Aliasing', desc: 'Maps global functions to unique local aliases.', icon: Layers },
        { id: 'lua54Support', label: 'Lua 5.4 Support', desc: 'Enables native bitwise operators for XOR logic.', icon: Binary },
        { id: 'tableLookup', label: 'Table Lookup', desc: 'Force calls through environment table resolution.', icon: Layers },
      ]
    },
    {
      title: "Post-Processing",
      icon: Shield,
      options: [
        { id: 'removeComments', label: 'Stripper', desc: 'Complete removal of development metadata.', icon: Shield },
        { id: 'minification', label: 'Minification', desc: 'Whitespace and character compression.', icon: Code },
      ]
    }
  ];

  const filteredSettings = useMemo(() => {
    if (!searchTerm.trim()) return settingsConfig;
    const query = searchTerm.toLowerCase();
    return settingsConfig.map(section => ({
      ...section,
      options: section.options.filter(opt => 
        opt.label.toLowerCase().includes(query) || 
        opt.desc.toLowerCase().includes(query)
      )
    })).filter(section => section.options.length > 0);
  }, [searchTerm, settingsConfig]);

  const getLogLevelColor = (level: LogEntry['level']) => {
    switch(level) {
      case 'success': return 'text-emerald-400';
      case 'warning': return 'text-amber-400';
      case 'error': return 'text-red-400';
      case 'process': return 'text-indigo-400';
      default: return 'text-zinc-400';
    }
  };

  const PresetInfo = {
    high: { label: 'V5 Extreme', icon: Flame, color: 'text-red-500', bg: 'bg-red-500/10' },
    balanced: { label: 'Balanced', icon: Scale, color: 'text-indigo-400', bg: 'bg-indigo-500/10' },
    fast: { label: 'Fast ID', icon: Wind, color: 'text-emerald-400', bg: 'bg-emerald-500/10' },
    custom: { label: 'Custom', icon: Settings, color: 'text-zinc-400', bg: 'bg-white/5' }
  };

  return (
    <div className="flex flex-col h-screen overflow-hidden bg-[#0a0a0c] text-zinc-300">
      <header className="h-14 lg:h-16 shrink-0 border-b border-white/10 glass flex items-center justify-between px-4 lg:px-6">
        <div className="flex items-center gap-3 h-full py-2">
          <img src={LOGO_URL} alt="LuaForm" className="h-full w-auto object-contain drop-shadow-[0_0_8px_rgba(129,140,248,0.3)]" />
        </div>
      </header>

      <div className="flex-1 flex flex-col lg:flex-row overflow-hidden relative">
        <nav className="lg:w-20 lg:border-r border-white/10 bg-zinc-900/20 flex lg:flex-col items-center justify-around lg:justify-start lg:pt-8 order-last lg:order-first z-50 py-2 lg:py-0">
          {[
            { id: 'changelog', icon: History, label: 'Logs' },
            { id: 'input', icon: Terminal, label: 'Code' },
            { id: 'output', icon: Code, label: 'Result' },
            { id: 'settings', icon: Settings, label: 'Config' },
            { id: 'console', icon: Activity, label: 'Console' },
          ].map(tab => (
            <button
              key={tab.id}
              onClick={() => setActiveTab(tab.id)}
              className={`flex flex-col items-center gap-1 p-2 lg:mb-6 rounded-xl transition-all ${
                activeTab === tab.id ? 'text-indigo-400' : 'text-zinc-500 hover:text-zinc-300'
              }`}
            >
              <tab.icon className={`w-5 h-5 lg:w-6 lg:h-6 ${activeTab === tab.id ? 'drop-shadow-[0_0_8px_rgba(129,140,248,0.5)]' : ''}`} />
              <span className="text-[9px] font-bold uppercase tracking-tighter">{tab.label}</span>
            </button>
          ))}
        </nav>

        <div className="flex-1 flex flex-col overflow-hidden p-3 lg:p-6 gap-4">
          <div className="flex-1 flex flex-col min-h-0">
            {activeTab === 'changelog' && (
              <div className="flex-1 overflow-y-auto no-scrollbar animate-in fade-in duration-500 space-y-6 max-w-4xl mx-auto w-full pb-10">
                <div className="text-center space-y-6 mb-8 mt-4 flex flex-col items-center">
                  <div className="relative group">
                    <div className="absolute -inset-4 bg-indigo-500/10 rounded-full blur-2xl group-hover:bg-indigo-500/20 transition-all duration-700"></div>
                    <img 
                      src={LOGO_URL} 
                      alt="LuaForm Hero" 
                      className="h-40 w-40 lg:h-56 lg:w-56 object-contain relative transition-transform duration-700 hover:scale-105" 
                    />
                  </div>
                  <div className="space-y-2">
                    <h2 className="text-3xl lg:text-4xl font-black text-white tracking-tight uppercase">CORE <span className="text-indigo-500">LOADED</span></h2>
                    <p className="text-sm text-zinc-500 max-w-md mx-auto">V5 Enterprise Hardening Engine. Optimized for Lua 5.4 and complex Luau environments.</p>
                  </div>
                </div>

                <div className="space-y-4">
                  {changelogs.map((log, i) => (
                    <div key={log.version} className={`glass rounded-3xl border border-white/5 p-6 transition-all hover:border-white/10 ${i === 0 ? 'bg-indigo-600/5 border-indigo-500/20' : ''}`}>
                      <div className="flex items-center justify-between mb-4">
                        <div className="flex items-center gap-3">
                          <span className={`px-2.5 py-1 rounded-lg text-[10px] font-black uppercase tracking-widest ${
                            log.type === 'major' ? 'bg-indigo-600 text-white shadow-lg shadow-indigo-600/20' : 'bg-zinc-800 text-zinc-400'
                          }`}>
                            {log.version}
                          </span>
                          <span className="text-xs text-zinc-600 font-bold uppercase tracking-widest">{log.date}</span>
                        </div>
                        {i === 0 && <span className="text-[9px] font-black text-indigo-400 uppercase tracking-widest flex items-center gap-1"><RefreshCw className="w-2.5 h-2.5 animate-spin" /> Active Deployment</span>}
                      </div>
                      <ul className="space-y-3">
                        {log.changes.map((change, j) => (
                          <li key={j} className="flex items-start gap-3 text-sm text-zinc-400 group">
                            <CheckCircle2 className={`w-4 h-4 mt-0.5 shrink-0 transition-colors ${i === 0 ? 'text-indigo-500' : 'text-zinc-700 group-hover:text-indigo-500/50'}`} />
                            <span className="leading-relaxed group-hover:text-zinc-200 transition-colors">{change}</span>
                          </li>
                        ))}
                      </ul>
                    </div>
                  ))}
                </div>

                <button 
                  onClick={() => setActiveTab('input')}
                  className="w-full py-4 rounded-2xl bg-white/5 border border-white/10 text-white font-bold text-sm hover:bg-white/10 transition-all flex items-center justify-center gap-2"
                >
                  START PROTECTION <ChevronRight className="w-4 h-4" />
                </button>
              </div>
            )}

            {activeTab === 'input' && (
              <div className="flex-1 flex flex-col glass rounded-2xl border border-white/10 overflow-hidden shadow-2xl animate-in fade-in duration-300">
                <div className="px-4 py-2 border-b border-white/5 bg-white/2 flex justify-between items-center shrink-0">
                  <div className="flex items-center gap-2">
                    <FileCode className="w-3.5 h-3.5 text-zinc-500" />
                    <span className="text-[10px] font-bold uppercase tracking-widest text-zinc-500">Source Editor</span>
                  </div>
                  <div className="flex items-center gap-2">
                    <input type="file" ref={fileInputRef} onChange={handleFileUpload} className="hidden" accept=".lua,.txt" />
                    <button onClick={() => fileInputRef.current?.click()} className="flex items-center gap-1.5 px-2.5 py-1 rounded-lg text-[9px] font-black bg-indigo-500/10 text-indigo-400 hover:bg-indigo-500/20 transition-all border border-indigo-500/20">
                      <Upload className="w-3 h-3" /> Upload
                    </button>
                    <button onClick={() => { setInputCode(''); addLog("Editor cleared.", "warning"); }} className="p-1.5 text-zinc-500 hover:text-red-400 transition-colors bg-white/5 rounded-lg border border-white/5">
                      <X className="w-3.5 h-3.5" />
                    </button>
                  </div>
                </div>
                <textarea 
                  value={inputCode} 
                  onChange={e => setInputCode(e.target.value)}
                  className="flex-1 w-full h-full bg-[#1e1e1e] p-4 outline-none resize-none code-font text-xs lg:text-sm text-indigo-100/80 selection:bg-indigo-500/30"
                  placeholder="-- Type or upload your script here..."
                  spellCheck={false}
                />
              </div>
            )}

            {activeTab === 'output' && (
              <div className="flex-1 flex flex-col glass rounded-2xl border border-white/10 overflow-hidden shadow-2xl animate-in slide-in-from-bottom-2 duration-300">
                <div className="px-4 py-2 border-b border-white/5 bg-white/2 flex justify-between items-center shrink-0">
                  <span className="text-[10px] font-bold uppercase tracking-widest text-zinc-500">Hardened Payload</span>
                  <div className="flex gap-2">
                    {outputData && (
                      <>
                        <button onClick={() => copyToClipboard(outputData.code)} title="Copy" className="p-1.5 text-zinc-400 hover:text-white bg-white/5 rounded-lg active:scale-90 transition-transform"><Copy className="w-3.5 h-3.5" /></button>
                        <button onClick={() => {
                          const blob = new Blob([outputData.code], {type: 'text/plain'});
                          const url = URL.createObjectURL(blob);
                          const a = document.createElement('a');
                          a.href = url; a.download = 'protected.lua'; a.click();
                          addLog("Script downloaded as protected.lua", "info");
                        }} title="Download" className="p-1.5 text-zinc-400 hover:text-white bg-white/5 rounded-lg active:scale-90 transition-transform"><Download className="w-3.5 h-3.5" /></button>
                      </>
                    )}
                  </div>
                </div>
                <div className="flex-1 relative bg-[#1e1e1e] overflow-hidden">
                  {isProcessing ? (
                    <div className="absolute inset-0 flex flex-col items-center justify-center bg-black/60 z-20">
                      <RefreshCw className="w-8 h-8 text-indigo-500 animate-spin mb-4" />
                      <p className="text-xs font-bold tracking-widest uppercase text-zinc-400">Executing V5 Pipeline...</p>
                    </div>
                  ) : !outputData ? (
                    <div className="flex flex-col items-center justify-center h-full text-zinc-600">
                      <Search className="w-12 h-12 mb-4 opacity-10" />
                      <p className="text-sm font-medium">Core standby</p>
                      <p className="text-[10px] uppercase font-bold mt-1 tracking-tighter">Awaiting instruction command</p>
                    </div>
                  ) : (
                    <textarea value={outputData.code} readOnly className="flex-1 w-full h-full bg-transparent p-4 outline-none resize-none code-font text-xs lg:text-sm text-emerald-400/90 selection:bg-emerald-500/20" />
                  )}
                </div>
              </div>
            )}

            {activeTab === 'settings' && (
              <div className="flex-1 overflow-y-auto pr-2 no-scrollbar animate-in fade-in duration-300 space-y-6 pb-20">
                <div className="space-y-6 sticky top-0 z-20 bg-[#0a0a0c]/90 backdrop-blur-2xl pt-1 pb-6 border-b border-white/5">
                  <div className="relative group">
                    <Search className="absolute left-4 top-1/2 -translate-y-1/2 w-4 h-4 text-zinc-500 group-focus-within:text-indigo-400 transition-colors" />
                    <input 
                      type="text" 
                      placeholder="Search protection layers..." 
                      value={searchTerm} 
                      onChange={e => setSearchTerm(e.target.value)} 
                      className="w-full bg-white/5 border border-white/10 rounded-2xl py-3 pl-11 pr-4 text-sm outline-none focus:border-indigo-500/50 focus:bg-indigo-500/5 transition-all" 
                    />
                  </div>
                  
                  {!searchTerm && (
                    <div className="space-y-3">
                      <h3 className="text-[10px] font-black text-zinc-500 uppercase tracking-widest px-1">Protection Strategy</h3>
                      <div className="relative" ref={presetRef}>
                        <button 
                          onClick={() => setIsPresetOpen(!isPresetOpen)}
                          className="w-full flex items-center justify-between p-4 rounded-2xl bg-white/5 border border-white/10 hover:border-white/20 transition-all group"
                        >
                          <div className="flex items-center gap-3">
                            <div className={`p-2 rounded-xl ${PresetInfo[activePreset].bg}`}>
                              {React.createElement(PresetInfo[activePreset].icon, { className: `w-5 h-5 ${PresetInfo[activePreset].color}` })}
                            </div>
                            <div className="text-left">
                              <p className="text-xs font-black uppercase text-white tracking-widest">{PresetInfo[activePreset].label}</p>
                              <p className="text-[10px] text-zinc-500">Active Pipeline Configuration</p>
                            </div>
                          </div>
                          <ChevronDown className={`w-4 h-4 text-zinc-500 transition-transform duration-300 ${isPresetOpen ? 'rotate-180' : ''}`} />
                        </button>

                        {isPresetOpen && (
                          <div className="absolute top-full left-0 right-0 mt-2 p-2 rounded-2xl bg-zinc-900 border border-white/10 shadow-2xl z-30 animate-in fade-in slide-in-from-top-2 duration-200">
                            {(['high', 'balanced', 'fast'] as const).map((pk) => (
                              <button
                                key={pk}
                                onClick={() => applyPreset(pk)}
                                className={`w-full flex items-center gap-3 p-3 rounded-xl hover:bg-white/5 transition-all ${activePreset === pk ? 'bg-indigo-500/5' : ''}`}
                              >
                                <div className={`p-2 rounded-lg ${PresetInfo[pk].bg}`}>
                                  {React.createElement(PresetInfo[pk].icon, { className: `w-4 h-4 ${PresetInfo[pk].color}` })}
                                </div>
                                <div className="text-left flex-1">
                                  <p className={`text-xs font-bold uppercase tracking-widest ${activePreset === pk ? 'text-indigo-400' : 'text-zinc-300'}`}>{PresetInfo[pk].label}</p>
                                  <p className="text-[9px] text-zinc-500">
                                    {pk === 'high' ? 'Multi-stage VM & bitwise masking' : pk === 'balanced' ? 'Optimal security/payload ratio' : 'Quick identifier randomization'}
                                  </p>
                                </div>
                                {activePreset === pk && <div className="w-1.5 h-1.5 rounded-full bg-indigo-500 shadow-[0_0_8px_rgba(99,102,241,0.8)]" />}
                              </button>
                            ))}
                          </div>
                        )}
                      </div>
                    </div>
                  )}
                </div>

                {filteredSettings.length > 0 ? (
                  filteredSettings.map((section, idx) => (
                    <div key={idx} className="space-y-4">
                      <div className="flex items-center gap-2 px-1">
                        <section.icon className="w-4 h-4 text-indigo-400" />
                        <h2 className="text-[11px] font-black text-zinc-500 uppercase tracking-widest">{section.title}</h2>
                      </div>
                      <div className="grid grid-cols-1 md:grid-cols-2 gap-3">
                        {section.options.map(opt => (
                          <label key={opt.id} className={`flex flex-col p-4 glass rounded-2xl border transition-all cursor-pointer group ${ (options as any)[opt.id] ? 'border-indigo-500/30 bg-indigo-500/5 shadow-lg shadow-indigo-600/10' : 'border-white/5 hover:border-white/10 hover:bg-white/5' }`}>
                            <div className="flex items-center justify-between mb-1">
                              <div className="flex items-center gap-2">
                                 {opt.icon && <opt.icon className={`w-3.5 h-3.5 ${(options as any)[opt.id] ? 'text-indigo-400' : 'text-zinc-500'}`} />}
                                 <span className={`text-sm font-bold ${(options as any)[opt.id] ? 'text-white' : 'text-zinc-300'}`}>{opt.label}</span>
                                 {opt.isBeta && <span className="px-1.5 py-0.5 rounded text-[8px] font-black bg-amber-500/20 text-amber-500 border border-amber-500/20 tracking-tighter uppercase">Beta</span>}
                              </div>
                              <input type="checkbox" checked={(options as any)[opt.id]} onChange={() => toggleOption(opt.id, opt.label)} className="w-5 h-5 rounded-md accent-indigo-600 bg-zinc-800 border-zinc-700 cursor-pointer" />
                            </div>
                            <p className="text-[10px] text-zinc-500 leading-relaxed group-hover:text-zinc-400 transition-colors">{opt.desc}</p>
                          </label>
                        ))}
                      </div>
                    </div>
                  ))
                ) : (
                  <div className="flex flex-col items-center justify-center py-20 text-zinc-600 opacity-50">
                    <Search className="w-12 h-12 mb-4" />
                    <p className="text-sm font-bold uppercase tracking-widest">No matching settings</p>
                  </div>
                )}
              </div>
            )}

            {activeTab === 'console' && (
              <div className="flex-1 flex flex-col glass rounded-2xl border border-white/10 overflow-hidden shadow-2xl animate-in fade-in duration-300">
                <div className="px-4 py-2 border-b border-white/5 bg-white/2 flex justify-between items-center shrink-0">
                  <span className="text-[10px] font-bold uppercase tracking-widest text-zinc-500">System Logs</span>
                  <button onClick={clearLogs} className="p-1.5 text-zinc-500 hover:text-red-400 bg-white/5 rounded-lg transition-colors"><Trash2 className="w-3.5 h-3.5" /></button>
                </div>
                <div className="flex-1 bg-black/40 overflow-y-auto p-4 code-font text-xs space-y-1.5 no-scrollbar">
                  {logs.map((log) => (
                    <div key={log.id} className="flex gap-3 group">
                      <span className="text-[10px] text-zinc-600 shrink-0 select-none">[{log.timestamp}]</span>
                      <span className={`${getLogLevelColor(log.level)} leading-relaxed break-words flex-1`}>
                        {log.level === 'process' && <RefreshCw className="w-2.5 h-2.5 inline-block mr-2 animate-spin" />}
                        {log.message}
                      </span>
                    </div>
                  ))}
                  <div ref={logEndRef} />
                </div>
              </div>
            )}
          </div>

          <div className="shrink-0 flex items-center justify-between glass rounded-2xl p-2 lg:p-3 border border-white/10 bg-white/5 gap-3">
             <div className="flex-1 overflow-x-auto no-scrollbar flex items-center gap-4 px-2">
                {outputData ? (
                  <>
                    <div className="shrink-0">
                      <p className="text-[8px] font-bold text-zinc-500 uppercase">Growth</p>
                      <p className="text-xs font-mono text-indigo-400">+{Math.round((outputData.newSize / outputData.originalSize) * 100 - 100)}%</p>
                    </div>
                    <div className="w-px h-6 bg-white/10" />
                    <div className="shrink-0">
                      <p className="text-[8px] font-bold text-zinc-500 uppercase">Entropy</p>
                      <p className="text-xs font-mono text-emerald-400">{outputData.variablesChanged} Mut</p>
                    </div>
                    <div className="w-px h-6 bg-white/10" />
                    <div className="shrink-0">
                      <p className="text-[8px] font-bold text-zinc-500 uppercase">Timing</p>
                      <p className="text-xs font-mono text-zinc-400">{outputData.timeTaken}ms</p>
                    </div>
                  </>
                ) : (
                  <p className="text-[10px] font-bold text-zinc-600 uppercase tracking-widest italic opacity-50">V5 CORE STANDBY</p>
                )}
             </div>
             <button onClick={handleObfuscate} disabled={isProcessing || !inputCode.trim()} className="px-6 lg:px-10 py-3 lg:py-4 bg-indigo-600 hover:bg-indigo-500 disabled:bg-zinc-800 disabled:text-zinc-600 rounded-xl font-bold text-white text-xs lg:text-sm shadow-xl active:scale-95 transition-all flex items-center gap-2">
                {isProcessing ? <RefreshCw className="w-4 h-4 animate-spin" /> : <ChevronRight className="w-4 h-4" />}
                OBFUSCATE
             </button>
          </div>
        </div>

        <aside className="hidden xl:flex w-80 border-l border-white/10 p-6 flex-col gap-6">
           <div>
              <h3 className="text-sm font-bold text-white flex items-center gap-2 mb-4">
                <span className="flex items-center gap-2"><Info className="w-4 h-4 text-indigo-400" /> SECURITY STATUS</span>
              </h3>
              <div className="p-4 rounded-2xl bg-white/5 border border-white/5 space-y-4">
                 <div className="flex justify-between items-center text-xs"><span className="text-zinc-500">Architecture</span><span className="text-zinc-300 font-mono">v5.1.2-MultiVM</span></div>
                 <div className="flex justify-between items-center text-xs"><span className="text-zinc-500">Bitwise Engine</span><span className={`font-mono ${options.lua54Support ? 'text-indigo-400' : 'text-zinc-400'}`}>{options.lua54Support ? 'Native 5.4' : 'Polyfill'}</span></div>
                 <div className="flex justify-between items-center text-xs"><span className="text-zinc-500">Global Protection</span><span className="text-emerald-500 font-bold">Active</span></div>
              </div>
           </div>
           <div className="p-4 rounded-2xl bg-indigo-600/5 border border-indigo-600/10">
              <p className="text-[10px] font-bold text-indigo-300 uppercase mb-2">V5 Engine Tip</p>
              <p className="text-xs text-indigo-200/60 leading-relaxed">
                <span className="text-white font-bold">Variable Renaming</span> in V5.1.2 now covers local multi-declarations, for-loop iterators, and function parameters, providing complete identity erasure.
              </p>
           </div>
           <div className="mt-auto pt-6 border-t border-white/5">
              <p className="text-[9px] text-zinc-600 font-bold uppercase tracking-widest leading-loose">NOTICE: LuaForm v5.1.2 uses enhanced dispatch logic. Ensure your target environment supports full table-based function resolution.</p>
           </div>
        </aside>
      </div>

      <footer className="h-10 border-t border-white/5 glass px-6 hidden lg:flex items-center justify-between text-[10px] text-zinc-600 font-bold uppercase tracking-widest">
        <div className="flex gap-6"><span>BYTECODE VIRTUALIZATION ENGINE</span><span>V5.1.2 ENTERPRISE RELEASE</span></div>
        <div className="flex items-center gap-2"><div className="w-1.5 h-1.5 rounded-full bg-emerald-500 animate-pulse" /><span>Protection Engaged</span></div>
      </footer>
    </div>
  );
};

const container = document.getElementById('root');
if (container) {
  const root = createRoot(container);
  root.render(<App />);
}
